---
title: Express笔记
createTime: 2025/11/24 15:17:40
permalink: /blog/o4u5225d/
tags:
  - nodejs
  - express
---

## 全局异步路由异常处理

在原生 Express 中，如果路由是这样的：

```js
app.get("/", async (req, res) => {
    throw new Error("error")
})
```

throw 的错误不会走到全局错误处理中间件，因为 Express 的内部路由机制只捕获同步代码中的错误，
而 async 函数返回的是一个 Promise，其内部异常属于 Promise rejection，不会自动被 Express 处理。

### 解决方案

[express-async-errors](https://www.npmjs.com/package/express-async-error) 会覆写 Express 的路由处理逻辑，使 async 路由中抛出的异常自动传递给
`app.use((err, req, res, next) => {})`。

此包必须在路由注册之前加载,因为它需要修改 Express 内部的 prototype。

```js
import express from "express"
import "express-async-errors"

const app = express()

app.get("/", async (req, res) => {
    throw new Error("error")
})

app.use((err, req, res, next) => {
    res.status(500).send(err.message)
})

app.listen(3000)
```

## 使用 Pino 代替 console.log

[pino](https://github.com/pinojs/pino)

`console.log` 在 Node.js 中是同步操作，会阻塞事件循环，尤其在高并发场景下容易成为性能瓶颈。

Pino 使用 异步流式写入，将日志快速输出到文件或控制台，几乎不会阻塞主线程。

[benchmarks](https://github.com/pinojs/pino/blob/main/docs/benchmarks.md)

## 使用 ultimate-express 提升性能

[µExpress / Ultimate Express](https://github.com/dimdenGD/ultimate-express)

[benchmarks](https://github.com/dimdenGD/ultimate-express#performance-against-other-frameworks)

### 主要特点

兼容性：Ultimate Express 与 Express.js API 完全兼容，确保平稳过渡，不会破坏现有代码。
性能：Ultimate Express 基于 µWebSockets 构建，利用优化的路由和其他增强功能，在某些情况下可实现高达 11.78 倍的性能提升。
易于使用：迁移所需更改极少——只需将您现有的 Express 套餐替换为 Ultimate Express 即可。

### 与 Express.js 的区别

虽然 Ultimate Express 的设计初衷是作为 Express 的直接替代品，但两者之间仍存在一些关键区别：

- 区分大小写的路由：默认启用。
- 异步错误处理：通过新的配置选项简化。
- 优化请求体解析：默认情况下，仅对 POST、PUT 和 PATCH 方法读取请求体。
- 增强型 HTTPS 设置：将 uWS 选项直接集成到应用程序构造函数中。

### ultimate-express 捕获异步路由异常

express-async-errors - 不起作用，改用内部提供的

```js
app.set("catch async errors", true)
```

```js
import express from "ultimate-express"

const app = express()
app.set("catch async errors", true)

app.get("/", async (req, res) => {
    throw new Error("error")
})

app.use((err, req, res, next) => {
    res.status(500).send(err.message)
})

app.listen(3000)
```

## 使用 PM2 开启 Cluster 模式

PM2 是 Node.js 的 进程管理器，可实现：

- 守护进程运行（服务异常自动重启）
- 多进程模式（Cluster 模式，提高 CPU 利用率）
- 日志管理（标准输出、错误输出文件化）
- 热重载和负载均衡
- 适合生产环境高并发 Node.js 服务。

### 启动 Express 应用

#### 单进程模式

```bash
pm2 start app.js --name my-app
```

#### Cluster 模式（多进程，提高并发）

```bash
pm2 start app.js --name my-app -i max
```

-i max 表示自动创建与 CPU 核心数相同的进程     
Cluster 模式下，每个进程独立运行，提高吞吐量  