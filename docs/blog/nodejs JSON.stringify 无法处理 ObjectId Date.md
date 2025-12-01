---
title: nodejs JSON.stringify 无法处理 ObjectId Date
createTime: 2025/11/04 20:40:15
permalink: /blog/ggtyhzh7/
tags:
  - nodejs
---
### 背景

在物联网平台后端，每天会有大量设备上报数据。如果每条数据都直接写入 MongoDB，**写入性能会非常低**。

解决方案：

1. 使用 **Kafka 作为消息队列**，缓冲设备上报的数据。
2. 消费端使用 **MongoDB 的 bulkWrite** 批量写入，提高性能。

---

### 遇到的问题

Kafka 消息只能传输 **字符串**，而 bulkWrite 的操作中可能包含：

```js
{
  updateOne: {
    filter: {_id: new ObjectId(device_id)},
    update: {$set: { temperature: 22.5, lastActive: new Date() }},
    upsert: true
  }
}
```

使用普通 JSON 序列化会出问题：

* `new Date()` 和 `new ObjectId()` **无法被 JSON.stringify 正确处理**
* Kafka 消息传输时会丢失这些类型信息
* 导致 MongoDB 写入失败或数据错误

---

### 解决方案：EJSON

MongoDB 官方提供 **EJSON（Extended JSON）**，支持序列化 `Date`、`ObjectId` 等特殊类型，同时可以在消费者端反序列化还原。

#### 使用示例

```js
const { EJSON, ObjectId } = require('bson');

// 构建 bulkWrite 操作
const bulkOps = [
  {
    updateOne: {
      filter: { _id: new ObjectId(device_id) },
      update: { $set: { temperature: 22.5, lastActive: new Date() } },
      upsert: true
    }
  }
];

// 序列化成字符串发送到 Kafka
const kafkaMessage = EJSON.stringify(bulkOps);

// Kafka 消费端
const receivedOps = EJSON.parse(kafkaMessage);

// 批量写入 MongoDB
await mongoCollection.bulkWrite(receivedOps);
```

#### 优点

* 保留了 **Date** 和 **ObjectId** 类型信息
* 可以直接发送 bulkWrite 操作集合到 Kafka
* 避免每条数据单独写入，显著提升写入性能

---

### 数据流流程图

```
  +-------------------+
  | 物联网设备*n       |
  | 上报数据           |
  +---------+---------+
          |||||
          vvvvv
  +-------------------+
  |      Kafka        |
  | 消息队列缓冲数据  |
  +---------+---------+
            |
            v
  +-------------------+
  |    消费端服务      |
  | 解析 EJSON 消息    |
  +---------+---------+
            |
            v
  +-------------------+
  |    MongoDB        |
  | bulkWrite 批量写入 |
  +-------------------+
```
