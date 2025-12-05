---
title: VMware 能识别 USB 设备，但无法连接（灰色）的问题解决办法
createTime: 2025/01/02 00:00:00
permalink: /blog/p3q7u3t9/
tags:
  - VMware
---
>原帖: https://blog.csdn.net/lxhujinghui2011/article/details/124533017

在你的虚拟机安装目录下找到（虚拟机名字）.vmx文件，用记事本打开

然后找到下面这句，原来是FALSE，把它改成TRUE ，保存，OK。

```bash
usb.restrictions.defaultAllow="TRUE"
```
