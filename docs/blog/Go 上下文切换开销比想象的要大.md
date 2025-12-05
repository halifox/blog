---
title: Go 上下文切换开销比想象的要大
createTime: 2025/10/04 01:00:00
permalink: /blog/cmeiskrt/
tags:
  - Go
---
在近期的性能测试中，我主要关注 **VerneMQ** 的吞吐性能、资源占用情况，以及 **MQTT.js** 在高并发场景下的消息接收性能瓶颈。

为此，我使用 **Go** 语言编写了一个消息发送程序，用于持续向 VerneMQ 推送消息。最初测试时，程序默认使用所有 CPU 核心，可能导致本地其他服务受到干扰。

为了减少资源竞争，我将 `runtime.GOMAXPROCS` 设置为 `1`，结果意外发现发送速率从约 **27 万 QPS** 提升至 **35 万 QPS**。
进一步测试后，确认这一现象确实与 Go 的上下文切换开销有关。

---

### 核心代码

```go
package main

import (
    "fmt"
    "runtime"
    "time"

    mqtt "github.com/eclipse/paho.mqtt.golang"
)

func main() {
    runtime.GOMAXPROCS(1) // 控制同时运行的 OS 线程数
    opts := mqtt.NewClientOptions()
    opts.AddBroker("tcp://localhost:1883")
    opts.SetClientID("id")
    opts.SetUsername("username")
    opts.SetPassword("password")

    client := mqtt.NewClient(opts)
    if token := client.Connect(); token.Wait() && token.Error() != nil {
        panic(token.Error())
    }
    defer client.Disconnect(250)

    payload := `{"name":"test","value":123}`
    start := time.Now()
    for i := 0; i < 1000000; i++ {
        token := client.Publish("test/topic", 0, false, payload)
        _ = token
        token.Wait() // 等待发布完成
    }
    elapsed := time.Since(start)
    fmt.Printf("耗时: %s\n", elapsed)
}
```

---

### 运行结果

| 配置项                                           | 耗时          |
| :-------------------------------------------- | :---------- |
| 默认运行（无修改）                                     | 11.5768639s |
| 注释 `runtime.GOMAXPROCS(1)`                    | 15.2015674s |
| 注释 `token.Wait()`                             | 13.7588952s |
| 同时注释 `runtime.GOMAXPROCS(1)` 与 `token.Wait()` | 23.5422052s |

---

### 原因分析

* **默认调度行为**：Go 默认将 `GOMAXPROCS` 设为 CPU 核心数，允许 Goroutine 并行执行。
* **上下文切换开销**：当 `GOMAXPROCS > 1` 时，调度器会在多个线程间频繁切换 Goroutine，即使循环为顺序执行，也会带来额外的调度与锁竞争开销。
* **单线程优化**：当将 `GOMAXPROCS` 设置为 `1` 时，所有 Goroutine 在单线程中顺序运行，从而避免上下文切换导致的性能损失。
* **`token.Wait()` 的影响**：移除等待逻辑后耗时变化不规律，说明在高并发情况下，网络延迟或 MQTT 客户端内部缓冲机制可能成为性能瓶颈。

---

### 结论

* **GOMAXPROCS 并非越大越好**。
  在部分高频调用或无 I/O 阻塞的场景下，过多的线程调度可能带来额外负担。
* **合理设置线程数**，可有效降低上下文切换和锁竞争带来的性能损耗。
