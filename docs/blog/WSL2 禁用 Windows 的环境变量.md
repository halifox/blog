---
title: WSL2 禁用 Windows 的环境变量
createTime: 2025/01/03 00:00:00
permalink: /blog/gc06pgcq/
tags:
  - wsl
---
特别是那些以 C:\Windows、Path 等形式自动注入的变量

## 修改 WSL 全局配置
编辑（或创建）/etc/wsl.conf 文件：
```bash
sudo nano /etc/wsl.conf
```
在文件中加入以下内容：
```conf
[interop]
appendWindowsPath = false

[interop]
enabled = false
appendWindowsPath = false
```

保存后退出。
然后重启 WSL：
```bash
wsl --shutdown
```
再重新打开 WSL 即可。
此设置会禁止 WSL 自动将 Windows 的 PATH 注入到 Linux 环境中。

### 验证
执行：
```bash
echo $PATH
```
若不再出现 /mnt/c/Windows 等路径，则设置成功。