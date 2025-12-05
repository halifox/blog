---
title: WSL2 Docker 端口空闲但无法绑定
createTime: 2025/10/02 00:00:00
permalink: /blog/2bsm802m/
tags:
  - wsl
  - docker
---

## 情景

在 Windows 使用 **WSL2 + Docker** 时，有时会遇到容器无法绑定 **1883 端口**（例如 MQTT 服务端口）的问题。
奇怪的是，使用以下命令检查后发现该端口并没有被占用：

```bash
netstat -ano | findstr 1883
```

或在 Linux 子系统中：

```bash
sudo lsof -i:1883
```

结果均为空，说明 1883 端口并未被进程占用。
即使重启 Docker、WSL 或整台电脑，问题依旧存在。

---

### 原因分析

通过查询资料发现，问题不是端口被程序占用，而是 **Windows 系统自动保留了部分端口范围（Excluded Port Range）**。
这些被保留的端口无法被 Docker 映射使用，即便看似空闲。

---

### 解决方法

1. 停止并重新启动 NAT 服务：

   ```cmd
   net stop winnat
   net start winnat
   ```

2. 查看被系统保留的端口范围：

   ```cmd
   netsh interface ipv4 show excludedportrange protocol=tcp
   ```

   检查输出中是否包含 `1883`。

3. 若被保留，可临时释放；重启后如果再次被占用，可考虑调整系统的动态端口范围或避免使用该端口。

---

### 总结

当在 WSL2 + Docker 环境中出现 **端口明明空闲但无法绑定** 的情况，尤其是常用服务端口（如 1883、3306、8080 等），
优先检查 **系统保留端口列表**，并通过重启 `winnat` 服务刷新端口分配即可解决。
