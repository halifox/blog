---
title: 故障笔记：为什么 Hono zValidator 拿不到请求数据？
createTime: 2026/01/16 14:50:49
permalink: /blog/0o1gbcez/
---

# 为什么 Hono zValidator 拿不到请求数据？

### 1. 标准用法

在使用 `@hono/zod-validator` 时，标准的 JSON 校验写法如下：

```javascript
import { zValidator } from '@hono/zod-validator'

app.post(
  '/',
  zValidator('json', userSchema),
  (c) => {
    // 只有校验通过后，才能通过 c.req.valid 获取数据
    const data = c.req.valid('json')
    return c.json({ success: true, data })
  }
)

```

### 2. 踩坑实录

**问题描述：**
在一次开发中，发现接口始终返回校验失败（或 `data` 为空），但通过客户端日志观察，请求正文中确实包含了正确的 JSON 数据。

**排查结论：**
经过分析定位，原因是请求头中缺失了 **`'Content-Type': 'application/json'`**。

**核心原理：**
Hono 的 `zValidator('json', ...)` 中间件会严格检查请求的 `Content-Type`。

* 如果没有指定为 `application/json`，Hono 不会自动尝试解析请求正文。
* 结果导致中间件认为 Body 为空或格式非法，进而触发校验失败（Validation Failed）。

### 3. 经验总结

* **强校验特性**：`zValidator` 的第一个参数（如 `'json'` 或 `'form'`）不仅决定了如何校验，也决定了它去哪里找数据。
* **客户端要求**：使用 `json` 模式时，必须确保请求头包含 `Content-Type: application/json`。
* **排查思路**：当遇到“明明发了数据却校验不通过”时，**首要检查请求头**而非数据逻辑。
