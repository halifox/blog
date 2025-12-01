---
title: VMware 安装 ESXi 在此主机上不支持嵌套虚拟化
createTime: 2025/12/01 17:20:17
permalink: /blog/k2127u66/
---
>原帖: https://juejin.cn/post/7269023450340409359

# VMware Workstation 在此主机上不支持嵌套虚拟化。

## 原因分析：

得知VMware Workstation Pro 升级至15.5.6版本后，可以与Hyper-V兼容起来了。于是升级了。升级之后可以正常开启虚拟机。前几天，想试一试Windows沙盒，就是于是也安装起来了。测试安装成功。今天又再次打开了虚拟机，就报如上错误。VMware Workstation 在此主机上不支持嵌套虚拟化。，一提示到这个，我就想起来是可能是Hyper-V开启捣的鬼，百度了一大堆，有人说是windows电脑系统升级到2004版本的问题，需要重装系统之类的，也有人说是VMware 版本没有卸除干净，需要重装。额…怎么说呢，不想这么折腾。于是，下意识想到了，每次为什么要在内存那里选项中，开启虚拟化 Intel VT -x/RVI(V)呢，是不是现在windows兼容了，不需要在vm里开启虚拟化了。抱着试一试的心态，于是成功了。
