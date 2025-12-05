---
title: Flutter曾经流行的框架现在都怎么样了
createTime: 2025/12/05 09:05:14
permalink: /blog/1qiao7iq/
tags:
  - flutter
---

## 状态管理框架

### 🪦[Scoped Model](https://pub.dev/packages/scoped_model/versions) 2017年8月17日-2022年8月24日

早期流行,它的理念被后来的 Provider 吸收和取代

### 🪦[Redux](https://pub.dev/packages/redux) 2016年5月17日-2021年2月23日

从 React 社区移植，提供严格、可预测的状态管理，但因其在 Dart/Flutter 中的模板代码较多而逐渐被更简洁的方案取代。

### 🔥[Bloc / Cubit](https://pub.dev/packages/bloc) 2018年10月8日-

引入了事件 (Events) -> 状态 (States) 的严格分离架构，非常适合复杂的大型应用。Cubit 是 Bloc 后来推出的更轻量级版本。

### 🔥[Provider](https://pub.dev/packages/provider) 2018年10月19日-

对 InheritedWidget/ScopedModel 的彻底简化和优化，因其简洁性和高性能迅速普及，成为 Flutter 官方推荐的方案之一。

### 🔥[Flutter Hooks](https://pub.dev/packages/flutter_hooks) 2018年12月17日-

引入 React Hooks 概念，用于简化 StatefulWidget，解决单个 Widget 内部的本地状态和生命周期管理。

### 🪦[MobX](https://pub.dev/packages/mobx) 2018年12月27日-2024年12月16日

从 JavaScript 生态移植，采用响应式 (Observable) 编程模型，通过代码生成简化了状态管理。

### 🪦[Fish Redux](https://pub.dev/packages/fish_redux) 2019年3月7日-2023年12月8日

阿里巴巴闲鱼团队出品，基于 Redux 的组件化框架。

### 🧊[GetX](https://pub.dev/packages/Get) 不活跃 2019年11月14日-2025年2月13日

以其一体化（状态、路由、依赖注入）和极简 API 迅速在全球范围内获得大量用户，尤其受到追求开发效率的开发者喜爱。

但是实际开发中,因为getx内部长期持有context,导致带来了很多问题,已经不用很久了,也不记得会导致哪些问题.

### 🔥[Riverpod](https://pub.dev/packages/riverpod) 2020年6月23日-

Provider 的彻底重构。它解决了 Provider 在编译时安全性和测试方面的痛点，并消除了对 BuildContext 的依赖，被许多人认为是下一代状态管理的首选。

### 🔥[Signals](https://pub.dev/packages/signals) 2023年11月27日-

Signals 是 Flutter/Dart 状态管理领域一个相对较新的、极具影响力的概念，它借鉴了 SolidJS 和 Preact Signals 等响应式框架的设计理念。
它的目标是提供一种极简、高性能的状态管理方式，实现精细化响应式 (Fine-Grained Reactivity)。

## 数据库

### 🔥[sqflite](https://pub.dev/packages/sqflite) 2017年6月28日-

Flutter 社区中最常用、最流行的 SQLite 插件。它提供了类似 Android 中 SQLiteOpenHelper 的 API 接口，用于执行原始 SQL 命令。

### 🔥[floor](https://pub.dev/packages/floor) 2019年3月9日-

一个健壮的对象关系映射 (ORM) 库，它基于 sqflite。
它允许您使用 Dart 对象和注解来定义数据模型和数据库操作，无需手写大量 SQL，类似于 Android 的 Room 库。

### 🔥[drift/moor](https://pub.dev/packages/drift) 2021年10月11日-

另一个功能强大的 ORM 库，它也基于 sqflite。它提供类型安全的 SQL 查询和强大的代码生成功能，支持 Dart Streams 以监听数据变化。

### 🪦[isar](https://pub.dev/packages/isar) 2020年6月5日-2023年4月25日

高性能、跨平台的 NoSQL 数据库。它被设计为现代 Flutter 应用的首选数据库，提供零拷贝序列化和强大的查询功能，速度非常快。

### 🧊[isar_community](https://pub.dev/packages/isar_community) 不活跃 2025年8月12日-

原版的 Isar 库在一段时间内,更新和维护出现停滞,Flutter 社区中的其他开发者接手并创建及其相关的依赖包,但是维护不活跃

### 🪦[hive](https://pub.dev/packages/hive) 2019年6月18日-2022年6月30日

一个轻量级、快速的键值对和NoSQL数据库，不需要原生依赖。它非常适合存储简单对象和大量数据。易于使用，但不支持复杂的查询。

### 🔥[sembast](https://pub.dev/packages/sembast) 2014年12月17日-

一个NoSQL 文档存储数据库，基于 Dart 编写，支持 Web 和原生平台。它提供灵活的查询功能。

### 🔥[ObjectBox](https://pub.dev/packages/ObjectBox) 2019年10月31日-

一个高性能、ACID 兼容的嵌入式 NoSQL 数据库。它声称比 SQLite 更快，并提供直观的 Dart API 来直接存储对象，无需 SQL 或 ORM 转换。

### 🔥[Realm](https://pub.dev/packages/Realm) 2023年2月7日-

由 MongoDB 提供支持，是一个面向对象、移动优先的数据库。它以高性能和易用性著称，