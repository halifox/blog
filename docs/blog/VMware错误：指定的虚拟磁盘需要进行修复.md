---
title: 故障笔记：VMware错误：指定的虚拟磁盘需要进行修复
createTime: 2025/10/01 00:00:00
permalink: /blog/h2uyfesi/
tags:
  - VMware
---
>原帖: https://www.cnblogs.com/fumin0622/p/14835668.html

使用VMware的时候由于内存不足意外卡死，再想扩展内存的时候发现提示指定的虚拟磁盘需要进行修复，既然虚拟磁盘损坏了，那就修复就行了。

1.打开命令提示符，cd到VMware安装路径  
win10默认安装路径：`C:\Program Files (x86)\VMware\VMware Workstation`  
例如：`cd C:\Program Files (x86)\VMware\VMware Workstation`  
如果自定义安装路径，那就需要自己找了。

2.找到你需要修复的虚拟磁盘路径  
例如我的虚拟磁盘路径为：`D:\Virtual Machines\Ubuntu\ubuntu 16.04.vmx`  
ubuntu 16.04.vmx，这个就是你虚拟机的配置文件

3.执行命令进行修复  
`vmware-vdiskmanager -R "虚拟磁盘路径"`  
例如我的就是：`vmware-vdiskmanager -R "D:\Virtual Machines\Ubuntu\Ubuntu 64 位-disk1-cl1.vmdk"`

还有另外一种情况，虚拟机卡死，再次打开的时候提示“该虚拟机似乎正在使用中。如果该虚拟机未在使用，请按“获取所有权(T)”按钮获取它的所有权。 否则，请按“取消©”按钮以防损坏”。  
这种情况直接进到虚拟磁盘里，里面有好像是四个后缀是.lck的文件，删掉就行了