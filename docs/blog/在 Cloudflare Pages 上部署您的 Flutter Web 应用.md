---
title: 在 Cloudflare Pages 上部署您的 Flutter Web 应用
createTime: 2025/01/03 00:00:00
permalink: /blog/cynb75iz/
tags:
  - Cloudflare
---

>原帖:https://sarthaknp.medium.com/deploying-your-flutter-web-app-on-cloudflare-pages-0d4a30caa99f

欢迎各位 Flutter 开发者！🚀 我们将开启一段激动人心的旅程，助您将 Flutter Web 应用部署到支持 CI/CD 的 Cloudflare Pages
上。整个过程只需不到五分钟。让我们一起深入探索，让您的应用脱颖而出！

首先，关于 Cloudflare 页面
Cloudflare Pages 为部署静态网站和 JAMstack 应用提供了便捷的解决方案。凭借对 CI/CD 流水线的内置支持，开发人员可以自动化部署流程，确保无缝更新和持续集成。

第一步：登录：
首先，让我们登录 Cloudflare 帐户。只需访问 Cloudflare 网站，然后像往常一样登录即可。如果您还没有帐户，注册是免费的。

步骤 2：访问 Cloudflare 页面：
登录后，找到 Cloudflare 控制面板。找到“页面和工作进程”部分，点击即可访问 Cloudflare Pages。

Cloudflare 控制面板
步骤 3：创建您的应用程序：
现在，让我们为我们的应用腾出一些数字空间。点击“创建应用”，见证您的项目迈向上线的第一步。

步骤 4：与 Git 集成：
现在是时候将我们的应用连接到 Git 代码库了。点击“连接到 Git”，然后根据您的喜好选择 GitHub 或 GitLab。

第五步：选择您的项目：
点击所需项目，然后点击“开始设置”以启动该过程并为接下来的旅程做好准备。

步骤 6：构建应用程序：
这是整个过程中最关键的一步。复制并粘贴提供的命令来构建你的应用。确保输出目录设置为“build/web”。

```bash
if cd flutter; then git pull && cd .. ; else git clone https://github.com/flutter/flutter.git; fi && ls &&
flutter/bin/flutter doctor && flutter/bin/flutter clean && flutter/bin/flutter config --enable-web &&flutter/bin/flutter
build web --web-renderer canvaskit --release
```

步骤 7：监控构建进度：
请您放松休息，Cloudflare 将为您完成构建。请留意构建日志以跟踪进度。一切就绪后，点击“继续项目”。

第 8 步：构建完成并可以通过生成域名访问：