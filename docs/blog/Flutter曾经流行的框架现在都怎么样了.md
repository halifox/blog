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

优点: 企业级标准；逻辑与 UI 解耦最彻底；测试性极佳（因为输入输出都是纯数据）；生态系统完善（IDE 插件、日志工具）。

缺点: 样板代码较多（虽然 Cubit 简化了很多）；对于简单页面来说显得“过度设计”；新手上手门槛稍高。

### 🔥[Provider](https://pub.dev/packages/provider) 2018年10月19日-

对 InheritedWidget/ScopedModel 的彻底简化和优化，因其简洁性和高性能迅速普及，成为 Flutter 官方推荐的方案之一。

优点: 简单易学；不仅是状态管理，更是依赖注入 (DI) 工具；与 Flutter 原生 API 贴合度高。

缺点: 严重依赖 BuildContext；不是编译时安全的（如果树中没有对应的 Provider，会抛出运行时错误
ProviderNotFoundException）；监听机制相对粗糙（Consumer 有时会写得比较繁琐）。

### 🔥[Flutter Hooks](https://pub.dev/packages/flutter_hooks) 2018年12月17日-

引入 React Hooks 概念，用于简化 StatefulWidget，解决单个 Widget 内部的本地状态和生命周期管理。

优点: 消灭 StatefulWidget 样板代码；逻辑复用性极强（可以将 controller 的创建、销毁逻辑封装在一个 hook 函数中）；UI 代码非常紧凑。

缺点: “黑魔法”（违背了 Flutter 显式的 Widget 构建逻辑）；严重依赖 Hook 的调用顺序（不能在条件判断中使用）；增加了团队理解成本。

### 🪦[MobX](https://pub.dev/packages/mobx) 2018年12月27日-2024年12月16日

从 JavaScript 生态移植，采用响应式 (Observable) 编程模型，通过代码生成简化了状态管理。

### 🪦[Fish Redux](https://pub.dev/packages/fish_redux) 2019年3月7日-2023年12月8日

阿里巴巴闲鱼团队出品，基于 Redux 的组件化框架。

### 🧊[GetX](https://pub.dev/packages/Get) 不活跃 2019年11月14日-2025年2月13日

以其一体化（状态、路由、依赖注入）和极简 API 迅速在全球范围内获得大量用户，尤其受到追求开发效率的开发者喜爱。

优点: 开发速度快，代码行数少；功能全面（"瑞士军刀"）。

缺点: 

- Context 滥用与内存泄漏: GetX 经常宣传 "不需要 Context"。实际上，它通过全局变量持有了
  Context/NavigatorState。这导致Controller 的生命周期与 Widget 树脱钩。如果在页面销毁后，异步任务尝试通过 GetX 操作 UI（例如显示
  Dialog），可能会引用已销毁的 Context，导致崩溃或内存泄漏。
- 反模式 (Anti-patterns): 它是全局可变状态的集合，这让单元测试变得非常困难（因为难以隔离测试环境）；自定义的路由管理（Get.to）脱离了
  Flutter 原生路由体系，导致与 Deep Linking 或其他路由库集成时出现各种 Bug。
- 维护风险: 作者经常在没有详细 Changelog 的情况下引入破坏性更新，且长期由单人主导，社区协作性差。

### 🔥[Riverpod](https://pub.dev/packages/riverpod) 2020年6月23日-

Provider 的彻底重构。它解决了 Provider 在编译时安全性和测试方面的痛点，并消除了对 BuildContext 的依赖，被许多人认为是下一代状态管理的首选。

优点: 编译时安全（不会出现 ProviderNotFoundException）；不依赖 BuildContext（可以在纯 Dart
逻辑中使用）；对异步数据（AsyncValue）的处理极其优雅；自动处理状态销毁和缓存。

### 🔥[Signals](https://pub.dev/packages/signals) 2023年11月27日-

Signals 是 Flutter/Dart 状态管理领域一个相对较新的、极具影响力的概念，它借鉴了 SolidJS 和 Preact Signals 等响应式框架的设计理念。
它的目标是提供一种极简、高性能的状态管理方式，实现精细化响应式 (Fine-Grained Reactivity)。

底层实现: 基于 DAG (有向无环图) 追踪依赖关系。当一个 Signal 变化时，只有依赖它的计算属性或 Effect 会重新运行。在 Flutter
中，通常配合 Watch Widget 使用。

优点: 性能极高（实现真正的细粒度更新，例如只更新 Text 组件中的字符串，而不重建整个组件树）；API 简单直观；自动依赖追踪（无需手动声明监听）。

## 数据库

### 🔥[sqflite](https://pub.dev/packages/sqflite) 2017年6月28日-

Flutter 社区中最常用、最流行的 SQLite 插件。它提供了类似 Android 中 SQLiteOpenHelper 的 API 接口，用于执行原始 SQL 命令。

底层实现:   
利用 Flutter 的 Platform Channels (平台通道) 调用 Android 和 iOS 原生的 SQLite API   
在 Web 和桌面端（Windows/Linux/macOS）通常需要配合 sqflite_common_ffi 使用，通过 FFI (外部函数接口) 调用 SQLite 库。

优点: 生态系统极其成熟，稳定性高；标准 SQL 语法，学习成本低；大量现成的 SQL 教程和资源可用。

缺点: 需要手写 SQL 语句，容易出现拼写错误且仅在运行时发现（非类型安全）；由于使用 Platform Channels，大量数据传输时可能存在性能瓶颈（FFI
版本稍好）；样板代码（Boilerplate）较多。

### 🧊[floor](https://pub.dev/packages/floor)不活跃 2019年3月9日-2024年5月8日

一个健壮的对象关系映射 (ORM) 库，它基于 sqflite。
它允许您使用 Dart 对象和注解来定义数据模型和数据库操作，无需手写大量 SQL，类似于 Android 的 Room 库。

### 🔥[drift/moor](https://pub.dev/packages/drift) 2021年10月11日-

另一个功能强大的 ORM 库，它也基于 sqflite。它提供类型安全的 SQL 查询和强大的代码生成功能，支持 Dart Streams 以监听数据变化。

### 🪦[hive](https://pub.dev/packages/hive) 2019年6月18日-2022年6月30日

一个轻量级、快速的键值对和NoSQL数据库，不需要原生依赖。它非常适合存储简单对象和大量数据。易于使用，但不支持复杂的查询。

底层实现: 纯 Dart 实现的追加型（Append-only）文件系统。数据以二进制形式存储在单个文件中。

优点: 无原生依赖（纯 Dart，不增加包体积，随处运行）；API 极其简单（类似 Map）；读写速度快。

缺点: 内存占用高（默认需将整个 Box 的索引/Key 加载到 RAM 中，不适合大数据量）；查询功能非常弱。

### 🪦[isar](https://pub.dev/packages/isar) 2020年6月5日-2023年4月25日

高性能、跨平台的 NoSQL 数据库。它被设计为现代 Flutter 应用的首选数据库，提供零拷贝序列化和强大的查询功能，速度非常快。

底层实现: 基于 C++ 编写的定制核心 (基于 MDBX)，通过 Dart FFI 直接调用。

优点: 速度极快（尤其是读取）；拥有出色的查询 API（组合查询、全文搜索）；支持 ACID 事务；API 设计非常现代且易用。

### 🧊[isar_community](https://pub.dev/packages/isar_community) 不活跃 2025年8月12日-

原版的 Isar 库在一段时间内,更新和维护出现停滞,Flutter 社区中的其他开发者接手并创建及其相关的依赖包

社区尝试修复原版 Isar 在新版 Flutter/Dart 中的构建错误。目前仍不活跃，且由不同的社区成员零散维护，稳定性和长期支持存在巨大风险，生产环境慎用。

### 🔥[sembast](https://pub.dev/packages/sembast) 2014年12月17日-

一个NoSQL 文档存储数据库，基于 Dart 编写，支持 Web 和原生平台。它提供灵活的查询功能。

底层实现: 纯 Dart 实现。通常将数据以 JSON 格式（或其他编码）写入文件。支持在内存中运行。

优点: 纯 Dart（无原生桥接问题）；极其灵活；支持事务；可以在任何 Dart 运行的地方运行；离线/单机应用非常可靠。

缺点: 性能不如 C++底层库（如 Isar/ObjectBox/Realm）；大数据量下加载和查询较慢（通常需要将数据加载到内存中操作）。

### 🔥[ObjectBox](https://pub.dev/packages/ObjectBox) 2019年10月31日-

一个高性能、ACID 兼容的嵌入式 NoSQL 数据库。它声称比 SQLite 更快，并提供直观的 Dart API 来直接存储对象，无需 SQL 或 ORM 转换。

底层实现: 基于 C/C++ 编写的高性能核心，通过 Dart FFI 调用。

### 🔥[Realm](https://pub.dev/packages/Realm) 2023年2月7日-

由 MongoDB 提供支持，是一个面向对象、移动优先的数据库。它以高性能和易用性著称，

底层实现: 基于 C++ (Realm Core)，通过 Dart FFI 调用。