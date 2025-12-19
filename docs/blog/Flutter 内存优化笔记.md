---
title: Flutter 内存优化笔记
createTime: 2025/12/05 00:00:00
permalink: /blog/1792q0tp/
tags:
  - flutter
---

## Flutter 内存管理机制

我们关注的内存一般可以分为三大块，包括应用整体内存、Dart 堆和栈内存、External 内存。

### 应用整体内存

包括客户端平台内存，以及 Flutter 引擎和 Dart-VM 自身的内存。通过整体内存的变化，可高效直观地判断功能模块是否有问题。但是不足就是，无法精确定位出问题的代码。

### Dart堆和栈内存

Dart 以 Isolate 划分独立的线程、堆和栈内存，不同 Isolate 是隔离的，并且是各自独立 GC 的。其中堆内存划分成两个空间：新生代内存和老生代内存空间。

新生代(New Generation)  内存空间较小，划分为等分的两部分，采用复制-清除法管理内存，效率高，执行频率比老生代高一些。经过一轮复制清除后，存活下来的对象会被转移到老生代空间。

老生代(Old Generation)  内存空间较大，内存管理要经历 标记 - 清除 - 整理 三个步骤。

标记算法采用对象可达性算法， GC root 维护了一个根对象列表，从这些跟对象出发，遍历所有可达对象，标记活动对象，这样就可以识别出没被标记的待清理的对象。

内存清理之后会出现零散的内存碎片，需要移动整理来腾出完整的可用内存池。进行清除和整理的时候，会使引擎所有线程都停止处理 -
‘stop the world’。

从标记到内存清除和整理， GC 是一个耗时的执行过程，虽然引擎自身也做了一些优化，如多线程并行执行、增量执行、闲时执行，以减少 GC
的性能影响。从开发的角度来说，从实现细节去减少内存占用，以及开发完功能后进行内存泄漏排查，是必不可少的开发步骤。

### External内存

原生对象占用的内存空间，如文件、解码的图片数据，虽不属于 Dart 的内存，但通过嵌入层把这些对象包裹成 Dart 可访问的对象，供 Dart
侧访问和操作。这块内存虽然不会影响 Dart 运行性能，但是出问题很容易导致 OOM，通常 Dart 对象引用未正常释放也会导致相关原生对象不回收。

## 内存泄漏的常见场景

引擎无法从业务侧自动判断哪些对象该不该清理，即使抛出 OOM 也不会强制清理，这就需要我们从编码细节上去主动规避，以下是一些常见导致内存泄漏的案例：

### 监听反注册缺失

排查内存泄漏的过程中，我们发现图片内存大幅度超出了图片缓存自身 size 限制的增长，并且不会被 GC
回收，经过排查发现我们封装的一个底层图片处理类，注册了图片事件流监听后，并没有在适当的时机做反注册处理。

在 dispose 方法中添加了反注册之后，图片内存就可以正常释放了。

### 长列表直接构建列表项

通过对列表数据遍历的方式，一次生成所有数据对应的 widget 列表，直接塞进 Column 里展示给用户，当加载了几页数据之后，数据量稍大就会轻易导致
OOM 或导致严重卡顿。

正确的处理方式是使用列表组件自带的滑窗创建列表项功能来动态创建列表项，如果列表结构比较复杂可以考虑使用 Sliver 系列组件。

### 延时、持续执行的闭包引用

Flutter 提供的延时和持续执行的对象有 Animation、Timer、Future 等，在结束执行之前，回调函数引用到的相关对象都会被强引用保留在内存中。

这种情况，需要确保在功能退出时做相关清理或结束执行的操作。

### 永久活动对象引用

除非需要永久保留或有明确的清理实现，否则不推荐将一些对象挂载到永久存活的对象下面。

如应用根节点实例化的 Provider model，常规我们都会把清理动作放置在 dispose 方法内执行，但应用根节点实例化的 model 应用周期内不会执行
dispose，这很容易让人忽略内存清理操作。

类似的，还有持有在单例对象属性和静态变量的对象，都需要配套功能退出后的清理操作。

### 操作抛错

进行退出清理的某行代码抛错后，后续的处理便不会执行到。

这种情况需要尽可能保证清理操作的健壮性，避免结束前抛异常。

### 第三方组件质量问题

做技术选型的时候，组件或方案的 Like 数量或 Git star 会作为质量参考的一个尺度，但实际情况即使是官方提供的库也还是会存在一些坑，如：

* video_player 视频组件，在注销之后内存并未全部释放
* shared_preference 数据量大了之后高频操作引起高 IO 内存问题

注意尽可能对引入组件进行性能和内存测试，避免功能开发完后才发现问题，增加维护成本。

### Flutter Engine 自身的问题

如 iOS 渲染 emoji 内存占用 +130 MB，且关闭页面无法被回收，目前只能从引擎内部去挖掘解决方案。

## 优化图片内存占用

图片的内存消耗计算公式通常为：

* $Memory = Width \times Height \times 4\text{ bytes (RGBA8888)}$

如果你在 UI 上只需要显示一个 100x100 的头像，但加载了一张 2000x2000 的原图，那么：

* 不优化： $2000 \times 2000 \times 4 \approx 16\text{MB}$
* 优化后： $100 \times 100 \times 4 \approx 40\text{KB}$

### 设置缓存宽高，图片会在解码阶段就被限制大小。

```
CachedNetworkImage(
  imageUrl: "https://example.com/large-image.jpg",
  // 关键属性：逻辑像素转为物理像素（通常乘以设备像素比）
  memCacheWidth: 300 * MediaQuery.of(context).devicePixelRatio.toInt(),
  memCacheHeight: 300 * MediaQuery.of(context).devicePixelRatio.toInt(),
) 
```

```
Image.network(
  imageUrl: "https://example.com/large-image.jpg",
  // 关键属性：逻辑像素转为物理像素（通常乘以设备像素比）
  memCacheWidth: 300 * MediaQuery.of(context).devicePixelRatio.toInt(),
  memCacheHeight: 300 * MediaQuery.of(context).devicePixelRatio.toInt(),
) 
```

### 调整 ImageCache 阈值

Flutter 默认的图片缓存是 100MB 或 1000 张图。对于内存吃紧的设备，可以适当调小：

```
void main() {
  PaintingBinding.instance.imageCache.maximumSizeBytes = 50 << 20; // 设置为 50MB
  PaintingBinding.instance.imageCache.maximumSize = 50; // 设置缓存张数
  runApp(MyApp());
}
```

### 针对长列表 (ListView) 的优化

在滚动长列表时，及时释放不可见图片的内存至关重要。

* `findChildIndexCallback`: 确保列表项能被正确回收。
* `addAutomaticKeepAlives: false`: 如果不需要保留状态，设置为 false。
* `addRepaintBoundaries: true`: 减少重绘开销。