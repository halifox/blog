---
title: VMware虚拟磁盘释放
createTime: 2025/12/01 17:29:49
permalink: /blog/p1nz9lvq/
tags:
  - VMware
---

> 原帖: https://zhuanlan.zhihu.com/p/141735961

```bash
# 在根分区写入一个充满 0 的临时大文件，用于把未使用的磁盘空间“清零”，为后续压缩做准备。
dd if=/dev/zero of=/zero.file bs=2M
# 强制把缓存中的数据同步写入磁盘，确保 /zero.file 完整写入。
sudo sync
# 删除临时填充文件，释放刚刚占用的空间，使这些空间变为“空白的零数据块”。
rm -rf /zero.file
# 调用 VMware Tools 对指定挂载点（此处为根分区 /）进行磁盘瘦身操作，压缩虚拟磁盘文件（如 .vmdk）。
sudo /usr/bin/vmware-toolbox-cmd disk shrink /
```