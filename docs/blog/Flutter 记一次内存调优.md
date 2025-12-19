---
title: Flutter 记一次内存调优
createTime: 2025/12/05 00:00:00
permalink: /blog/fgphhse2/
tags:
  - flutter
---

直接看有问题的代码，这段代码导致一个带有图片显示的ListView中使用了400MB+的内存.

```dart
import 'package:cached_network_image/cached_network_image.dart';

CachedNetworkImage(
  key: key,
  imageUrl: imageUrl,
  imageBuilder: (context, imageProvider) => Container(
    decoration: BoxDecoration(
      borderRadius: BorderRadius.circular(4),
      image: DecorationImage(image: imageProvider, fit: BoxFit.cover),
    ),
  ),
  fadeOutDuration: .zero,
  fadeInDuration: .zero,
  fit: .cover,
  width: width,
  height: height,
  memCacheWidth: (width * MediaQuery.of(context).devicePixelRatio).toInt(),
  memCacheHeight: (height * MediaQuery.of(context).devicePixelRatio).toInt(),
)
```

## 为什么使用`imageBuilder`来设置圆角

根据贡献者 renefloor 在issues中的[回复](https://github.com/Baseflow/flutter_cached_network_image/issues/610#issuecomment-907096019):

参照README.md的示例，使用`imageBuilder`实现圆形图像、方形图像或其他格式的图像。

## 为什么没有最开始没使用`ClipRRect`

`ClipRRect` 会在渲染树中添加一个裁剪层。它要求 GPU 先绘制子组件（矩形图片），然后应用一个裁剪遮罩，把多余的角切掉。\
在一些复杂的场景下(列表的高速滚动，动画叠加，嵌套裁剪，阴影与光效，大量的小图阵列)，裁剪可能会触发 Offscreen Layer（离屏渲染）。这意味着 GPU 需要开辟一块临时的缓冲区，画好后再合并回主屏幕。\
如果列表里有几十个 ClipRRect，这种层级合成（Compositing）会导致掉帧。

并且在很多Flutter性能调优的帖子中提到
> 尽量避免用Clipxxx组件，建议用BoxDecoration的image属性实现，如果用Clipxxx组件，圆角取整后性能会提升。

## 原因分析

当使用 `imageBuilder` 并返回一个 `Container` 时，实际上是把 `imageProvider` 交给了 `BoxDecoration` 再次处理。
在复杂的列表滑动中，这种重新包装可能会导致内存中的 `ImageStream` 无法被即时回收，

## 解决方案: 使用`ClipRRect`

“内存溢出导致崩溃” 比 “GPU 掉帧” 是更严重的生产事故。因此，回归 `ClipRRect` 是一种**“用少许 GPU 负载换取极大内存安全”**的务实策略。

```dart
ClipRRect(
  borderRadius: .circular(4),
  child: CachedNetworkImage(
    key: key,
    imageUrl: imageUrl,
    fadeOutDuration: .zero,
    fadeInDuration: .zero,
    fit: .cover,
    width: width,
    height: height,
    memCacheWidth: (width * MediaQuery.of(context).devicePixelRatio).toInt(),
    memCacheHeight: (height * MediaQuery.of(context).devicePixelRatio).toInt(),
  ),
);
```

## 补充：`ClipRRect` 的现代视角

Skia vs Impeller： 随着 Flutter 3.x 引入 Impeller 渲染引擎，离屏渲染（SaveLayer）的开销已经大幅降低。Impeller 能够以更小的代价处理裁剪。
