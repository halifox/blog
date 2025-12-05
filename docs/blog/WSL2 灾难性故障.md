---
title: "WSL2 错误代码: 灾难性故障:Wsl/Service/E_UNEXPECTED"
createTime: 2025/01/03 00:00:00
permalink: /blog/jbb6btjs/
tags:
  - wsl
---

## 解决方案

```bash
wsl --shutdown
net stop LxssManager
net start LxssManager
wsl
```

LxssManager（LXSS Manager Service）作用是管理 Windows Subsystem for Linux (WSL) 的核心运行，包括：

- 启动、停止 WSL 实例
- 管理 Linux 发行版生命周期
- 管理网络、文件系统桥接
- 驱动 wsl.exe 与实际 Linux 子系统之间的衔接