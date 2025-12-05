---
title: Flutter Web 应用的 Cloudflare Pages 部署教程
createTime: 2025/12/05 00:00:00
permalink: /blog/cynb75iz/
tags:
  - Cloudflare
---

>原帖:https://sarthaknp.medium.com/deploying-your-flutter-web-app-on-cloudflare-pages-0d4a30caa99f

1. 确保输出目录设置为“build/web”。


2. 构建命令
```bash
if cd flutter; then git pull && cd .. ; else git clone https://github.com/flutter/flutter.git; fi && ls &&
flutter/bin/flutter doctor && flutter/bin/flutter clean && flutter/bin/flutter config --enable-web &&flutter/bin/flutter
build web --web-renderer canvaskit --release
```