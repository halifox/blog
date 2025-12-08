---
title: Flutter 框架沉浮录
createTime: 2025/12/05 00:00:00
permalink: /blog/1qiao7iq/
tags:
  - flutter
---

## 状态管理框架

### 🪦[Scoped Model](https://pub.dev/packages/scoped_model) 2017年8月17日-2022年8月24日

早期流行,它的理念被后来的 Provider 吸收和取代

### 🪦[Redux](https://pub.dev/packages/redux) 2016年5月17日-2021年2月23日

从 React 社区移植，提供严格、可预测的状态管理，但因其在 Dart/Flutter 中的模板代码较多而逐渐被更简洁的方案取代。

### 🔥[Bloc / Cubit](https://pub.dev/packages/bloc) 2018年10月8日-

引入了事件 (Events) -> 状态 (States) 的严格分离架构，非常适合复杂的大型应用。Cubit 是 Bloc 后来推出的更轻量级版本。

优点: 逻辑与 UI 解耦最彻底；测试性极佳（因为输入输出都是纯数据）；生态系统完善（IDE 插件、日志工具）。

缺点: 样板代码较多（虽然 Cubit 简化了很多）；对于简单页面来说显得“过度设计”；新手上手门槛稍高。

### 🔥[Provider](https://pub.dev/packages/provider) 2018年10月19日-

对 InheritedWidget/ScopedModel 的彻底简化和优化，因其简洁性和高性能迅速普及，成为 Flutter 官方推荐的方案之一。

优点: 简单易学；不仅是状态管理，更是依赖注入 (DI) 工具；与 Flutter 原生 API 贴合度高。

缺点: 严重依赖 BuildContext；不是编译时安全的（如果树中没有对应的 Provider，会抛出运行时错误
ProviderNotFoundException）；监听机制相对粗糙（Consumer 有时会写得比较繁琐）。

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

### 🔥[Flutter Hooks](https://pub.dev/packages/flutter_hooks) 2018年12月17日-

引入 React Hooks 概念，用于简化 StatefulWidget，**解决单个 Widget 内部的本地状态和生命周期管理**。

flutter_hooks 的出现，使得所有基于 StatefulWidget 模式的框架，都面临着向**“函数式/声明式”**转变的压力。

Riverpod 官方推荐使用 hooks_riverpod 包。

BLoC 社区也提供了 flutter_bloc 的 Hooks 扩展。

优点: 消灭 StatefulWidget 样板代码；逻辑复用性极强（可以将 controller 的创建、销毁逻辑封装在一个 hook 函数中）；UI 代码非常紧凑。

缺点: “黑魔法”（违背了 Flutter 显式的 Widget 构建逻辑）；严重依赖 Hook 的调用顺序（不能在条件判断中使用）；增加了团队理解成本。

### 个人见解

**细粒度的自动依赖追踪**是未来的方向，它消除了“为了连接状态和 UI 而写的样板代码”

#### "Composition over Inheritance" (组合优于继承)

`BlocBuilder` (组合/嵌套) 比 `extends ConsumerWidget` (继承/侵入) 更好

* 低侵入性：使用 Builder 模式（如 BlocBuilder 或 Consumer），你的 Widget 依然是一个纯粹的 StatelessWidget。这意味着该 Widget
  的其余部分与特定框架解耦，迁移或复用更容易。

* 局部性：Builder 明确地圈定了“哪里需要重绘”。你看一眼代码就知道，只有包裹在 builder 里的那几行代码会动，而不是整个 Widget
  类。

* 显式的范围控制，而不是隐式的全局/类级别注入，这符合 Flutter 官方推荐的思维模式。

#### 依赖追踪：自动识别 (Auto-detect) vs 手动声明 (Manual Declaration)

`Watch()` 自动追踪状态，远胜于 `BlocBuilder<Bloc, State>` 的手动泛型声明。

* BLoC/Provider 不仅要写泛型，还要写 equatable 来比较 props，或者写 listenWhen/buildWhen 来过滤。这属于“命令式”的优化。

* 心智负担极低。你不需要告诉框架“由于 A 变了所以更新 B”，你只需要“使用 A”，框架会自动建立依赖关系。

* 这种模式在前端界（React Preact Signals, SolidJS, Vue）已经赢了。

#### 细粒度更新 vs 代码拆分成本

* 性能优化的悖论：Flutter 官方说“只重建或更新 UI 中真正发生变化的那一小部分”。但现实中，业务逻辑一变，你发现原先拆分的小
  Widget 需要多传一个参数，或者原先的 BlocBuilder 需要监听一个新的 State。

* 重构地狱：BLoC：如果你的 UI 突然需要展示 UserBloc 和 OrderBloc 的数据，你得把原本的 BlocBuilder<UserBloc...> 改成
  MultiBlocBuilder 或者嵌套，泛型改来改去，非常痛苦。

* 重构地狱：Riverpod：虽然 ref.watch 解决了组合问题，但为了性能（避免整个页面重绘），你还是得把局部 UI 拆成单独的 Widget
  类（ConsumerWidget），这增加了文件数量和代码跳转难度。

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

### 个人见解

#### 使用 SQLite 为底层的库

SQLite 作为最老牌、最成熟的嵌入式数据库，其底层引擎久经考验，完整支持 ACID 事务特性，是数据持久化的工业标准。其可靠性在所有主流平台（包括
Web 端的 SQL.js）上得到了验证。

SQL 语言是跨平台开发者的通用语言，iOS/Android 开发者对其高度熟悉，极大降低了学习和迁移成本。同时，SQLite
拥有丰富的第三方调试工具和数据查看器，调试和维护成本极低。

长远来看，选择基于 SQLite 的方案（如 drift 或 sqflite）是追求 **架构稳健性** 和 **低技术锁定风险** 的最佳战略选择。

#### 自研/NoSQL 数据库的风险评估

不可否认，Isar、ObjectBox、Realm 等现代数据库提供了极其便捷的开发体验和对象模型封装（如 Isar 的 @Collection
标注）。在纯粹的开发速度和特定场景的读写性能上，它们往往表现优异。

这类自研或专用底层核心的数据库（如 Isar 的 C 核心，ObjectBox 的自研存储引擎）存在巨大的 技术锁定风险 (Vendor Lock-in Risk)
。其底层机制对于大多数开发者而言是 不透明的黑盒。

Isar 原作者停止积极维护正是这一风险的有力证明。一旦核心维护者或公司放弃，社区接管难度极高，项目将面临高风险的技术停滞或弃用。

如果项目对 长期可持续性 和 技术栈通用性 要求较高，应警惕使用这类非标准化底层数据库带来的 核心维护依赖风险 。

## 依赖注入

### 🪦[kiwi](https://pub.dev/packages/kiwi) 2018年9月9日-2024年7月22日

### 🔥[get_it](https://pub.dev/packages/get_it) 2018年5月22日-

get_it 是 Dart 和 Flutter 社区中一个高度流行且备受推崇的服务定位器（Service Locator）。
它致力于提供一种简单、快速、类型安全的方式来管理应用程序中的依赖项（服务、模型和业务逻辑），并实现代码解耦。

原理： get_it 扮演一个全局的中央注册表（Service Locator）。
开发者在应用程序启动时，将所有服务及其创建方法（工厂）注册到这个中央注册表中。当应用程序的任何部分需要某个服务时，它就向这个定位器请求（“定位”）所需的实例。

### 🔥[injectable](https://pub.dev/packages/injectable) 2020年1月28日-

injectable 并不是一个独立的依赖注入（DI）解决方案，而是为 get_it 量身定制的“代码生成器”，
旨在将 get_it 的服务定位器（Service Locator）模式提升到基于注解和编译时检查的依赖注入容器的水平。

它极大地简化了 get_it 的手动注册流程，将大部分样板代码工作交给了机器处理。

## 网络请求

### 🔥dart:io HttpClient

Dart 语言的标准库 dart:io 中提供的 HTTP 客户端。

它是所有 Dart 应用程序（包括 Flutter）进行网络请求的最底层实现。

功能强大且灵活，但使用时需要手动处理请求头、响应流、错误处理等细节，代码量较大，一般不直接用于业务开发。

### 🔥[http](https://pub.dev/packages/http) 2012年11月30日-

Google 推荐的官方 HTTP 客户端库。简单、易用、轻量，但功能相对基础，常用于简单的 GET/POST 请求。

缺乏拦截器、全局配置等高级功能，文件上传/下载支持相对不便。

### 🔥[dio](https://pub.dev/packages/dio) 2018年4月21日-

社区中最流行、功能最强大的网络请求库。提供拦截器、全局配置、FormData、文件上传下载、取消请求等功能。

### 🔥[chopper](https://pub.dev/packages/chopper) 2018年4月7日-

结合 dio 或 http，通过代码生成技术，以声明式接口（类似 Java 的 Retrofit）定义 API 接口，大幅简化 API 调用代码。

Chopper 缺乏对文件上传/下载进度的原生支持，也不具备取消请求（Cancellation Token）的便捷机制。

需要通过自定义 Converter 接口处理 JSON 数据的序列化和反序列化。
但对于新手或习惯了 retrofit 流程的开发者来说，可能需要更多的配置工作来达到类型安全的自动模型转换。

相比于 dio + retrofit 这一组合，Chopper 的用户基础和社区生态相对较小。
这意味着当你遇到一些边缘或复杂的问题时，找到相关的中文或英文资料、示例代码或社区支持可能会更困难。

### 🔥[retrofit](https://pub.dev/packages/retrofit) 2019年4月7日-

它不仅在声明式接口定义上向 Android 的 Retrofit 看齐，更重要的是，它通过强大的代码生成，
弥补了 Chopper 在自动化数据模型转换方面的缺失，实现了请求与结果的无缝序列化/反序列化。

### 个人见解

我比较喜欢`retrofit`

## 序列化

### [dart:convert](https://dart.dev/libraries/dart-convert)

零依赖，Dart 标准库自带，简单快速，适用于极其简单的模型。

手动编写 `toJson()` 和 `fromJson()` 属于样板代码，代码冗余、繁琐。模型一旦复杂，容易出错且难以维护。

### [built_value](https://pub.dev/packages/built_value)

提供了最严格的不可变保证，强大的集合（List/Map）支持，能够创建非常一致和可靠的数据模型。

Dart 原生的 List 和 Map 是可变的。built_value 强制要求在模型中使用 BuiltList、BuiltMap 和 BuiltSet 这些专有的不可变集合类型。

当你在 Builder 中操作集合时，你实际上操作的是一个临时的、可变的集合（例如，原生的 List），一旦调用 build() 方法，
这些临时集合就会被锁定并转换为不可变的 BuiltList，附加到新的不可变模型实例上。

built_value 会自动生成 == 运算符和 hashCode，它们会递归地比较对象的所有属性和内部集合的内容，确保只有当两个对象的内容完全相同时才被认为是相等的。

由于模型不可变，你在代码库的任何地方传递数据时，都不必担心它会在不知情的情况下被另一个函数修改（即消除副作用），这极大地提高了代码的稳定性和可预测性，特别是在复杂的状态管理系统中。

built_value 通过强制性的设计模式（Builder）和特殊的不可变类型（Built Collections），从根本上防止了数据变异，从而实现了最严格、最可靠的数据模型保证。    

### [json_serializable](https://pub.dev/packages/json_serializable)

自动化生成 fromJson() / toJson() 代码，极大地减少了开发工作量，高效且类型安全。通过注解（@JsonKey）可以灵活处理字段命名、默认值、忽略字段等复杂需求。
    
绝大多数 Flutter/Dart 生产级应用的首选。是实现序列化最平衡和推荐的方式。

### [freezed](https://pub.dev/packages/freezed)

强制实现不可变性 (Immutability)，极大地减少了状态管理中的潜在 bug。自动生成 ==, hashCode, toString 和强大的 copyWith 方法。与 json_serializable 集成完美。

实现原理和`built_value`差不多,但是 freezed 自动生成了 copyWith 方法，这是唯一“修改”不可变对象的途径。

### 个人见解

还是希望出现和`kotlinx.serialization`一样功能强大,多格式,不依赖反射,易用的序列化库

## 多语言

