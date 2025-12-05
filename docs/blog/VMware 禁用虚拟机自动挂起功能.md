---
title: VMware 禁用虚拟机自动挂起功能
createTime: 2025/10/01 00:00:00
permalink: /blog/f2qwq9di/
tags:
  - VMware
---

> 原帖: https://blog.csdn.net/qq_45392321/article/details/117332517

### 在 VMware Workstation 中禁用虚拟机的挂起功能

1. 关闭虚拟机。

2. 找到虚拟机文件夹。

3. 在文本编辑器中打开 .vmx 文件以进行编辑。

4. 将下行添加到 .vmx 文件中：

    ```js
    suspend.disabled = "TRUE"
    ```

5. 保存并关闭 .vmx 文件。

6. 重新启动虚拟机。

