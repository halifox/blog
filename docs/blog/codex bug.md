---
title: windows登录Codex时遇到一个坑及解决方法，http://localhost:1455/auth/callback?code=ac_Tu…..，无法访问此网站 localhost 拒绝了我们的连接请求
createTime: 2025/11/28 10:55:36
permalink: /blog/tupexq35/
tags:
  - codex
---

>原帖: https://linux.do/t/topic/1019070

查看 1455 端口是否被排除：

```bash
netsh interface ipv4 show excludedportrange protocol=tcp
```

如果被排除了，重启 WinNAT 服务：

```bash
net stop winnat
net start winnat
```


诶 和[WSL2 + Docker 端口空闲但无法绑定](/blog/2bsm802m/)的原因是一样的