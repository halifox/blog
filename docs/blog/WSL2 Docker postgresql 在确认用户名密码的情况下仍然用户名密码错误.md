---
title: WSL2 Docker postgresql 在确认用户名密码的情况下仍然用户名密码错误
createTime: 2025/01/01 00:00:00
permalink: /blog/ol37x96n/
tags:
  - wsl
  - docker
---

## 情景

有时候,在我确认postgresql数据库账号密码和其他配置没有任何错误,即使重新创建全新的容器,仍然从宿主机上的数据库工具连接数据库,提示用户名密码错误.

## 排查

1. 在 PostgreSQL 容器内使用 psql 可正常连接，说明账号与密码均配置正确。
2. 同一网络下的其他容器可成功访问数据库，说明数据库已正常对外开放连接。
3. 将端口映射改为 "15432:5432" 后即可正常访问，初步判断问题出在 WSL 与 Windows 对 5432 端口的占用或冲突上。

## 解决

1. 端口检查

```bash
PS C:\Users\user> netstat -ano | findstr 5432
  TCP    127.0.0.1:3461         127.0.0.1:5432         TIME_WAIT       0
  TCP    127.0.0.1:5432         0.0.0.0:0              LISTENING       18120
  TCP    127.0.0.1:6338         127.0.0.1:5432         TIME_WAIT       0
  TCP    127.0.0.1:10093        127.0.0.1:5432         TIME_WAIT       0
  TCP    127.0.0.1:13158        127.0.0.1:5432         TIME_WAIT       0
  TCP    127.0.0.1:13159        127.0.0.1:5432         TIME_WAIT       0
  TCP    [::1]:5432             [::]:0                 LISTENING       18120
  UDP    0.0.0.0:3702           *:*                                    5432
  UDP    0.0.0.0:3702           *:*                                    5432
  UDP    0.0.0.0:57948          *:*                                    5432
  UDP    [::]:3702              *:*                                    5432
  UDP    [::]:3702              *:*                                    5432
  UDP    [::]:57949             *:*                                    5432
```

2. 查找进程

```bash
PS C:\Users\user> tasklist | findstr 18120
wslrelay.exe                 18120 Console                    1      2,728 K 
```

端口 5432 当前被进程 wslrelay.exe (PID 18120) 占用。

wslrelay.exe 是 Windows Subsystem for Linux (WSL) 的一个中间进程，用于在 Windows 和 WSL 间转发网络连接。

3. 进入wsl中查找

```bash
user@LAPTOP-0VETJ0H7:/mnt/c/Users/user$ sudo docker ps | grep 5432

user@LAPTOP-0VETJ0H7:/mnt/c/Users/user$ sudo lsof -i :5432
[sudo] password for user: 
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
docker-proxy 791 root    7u  IPv4  20938      0t0  TCP *:postgresql (LISTEN)
docker-proxy 797 root    7u  IPv6  20939      0t0  TCP *:postgresql (LISTEN)
```

`docker ps`命令没有输出,代表容器已停止

但 docker-proxy 仍在占用 5432，可能是 Docker 进程未正确清理映射。

4. 尝试重启docker与wsl,但是 docker-proxy 仍占用 5432，说明 Docker daemon 启动时残留了一个无容器绑定的端口映射。

5. 直接kill

```bash
user@LAPTOP-0VETJ0H7:/mnt/c/Users/user$ sudo ss -lptn | grep 5432
LISTEN 0      4096          0.0.0.0:5432      0.0.0.0:*    users:(("docker-proxy",pid=3093,fd=7))   
LISTEN 0      4096             [::]:5432         [::]:*    users:(("docker-proxy",pid=3100,fd=7))   
user@LAPTOP-0VETJ0H7:/mnt/c/Users/user$ sudo kill -9 3093 3100
```

6. 解决,现在重新启动postgresql容器可以正确连接数据库