---
title: VMware 磁盘扩容
createTime: 2025/10/01 00:00:00
permalink: /blog/5w36ovkb/
tags:
  - VMware
---
当在VMware里使用Linux的时候 需要对磁盘进行扩容

1. 关闭Linux
2. 在VMware设置里调整Linux磁盘大小
3. 打开Linux
4. 在Linux里安装gparted工具

```shell
sudo apt-get install gparted 
```

5. 使用gparted调整分区的大小

```shell
sudo gparted
```
