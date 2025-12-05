---
title: AOSP 9.0.0 编译与模拟器运行指南（Ubuntu 20.04）
createTime: 2025/10/05 00:00:00
permalink: /blog/38hrmb7t/
---

## 系统环境

首先，查看系统的基本信息：

```shell
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:    20.04
Codename:   focal
```

## 必要的依赖安装

为了开始编译 AOSP，我们首先需要安装一些必要的依赖包。使用以下命令安装所需的工具和库：

```shell
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig openssl libssl-dev
```

## 配置 Git 信息

配置你的 Git 用户名和邮箱，这将用于代码管理：

```shell
git config --global user.name ubuntu
git config --global user.email ubuntu@ubuntu.com
```

## 创建并进入 AOSP 项目目录

创建 AOSP 工作目录并进入该目录：

```shell
mkdir AOSP
cd AOSP
```

## 安装 Python 2.7

AOSP 编译过程需要 Python 2.7，因此我们需要安装并设置 Python 版本：

```shell
sudo apt install python2-minimal
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
sudo update-alternatives --config python
```

在选择 Python 版本时，选择 Python 2 作为默认版本。

## 安装 Repo 工具

AOSP 使用 `repo` 工具来管理代码仓库。我们需要安装 `repo` 工具：

```shell
mkdir -p ~/.bin
PATH="${HOME}/.bin:${PATH}"
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
chmod a+rx ~/.bin/repo
repo version
```

## 初始化 AOSP 项目

使用 `repo` 工具初始化 AOSP 项目并同步代码：

```shell
repo init -u https://android.googlesource.com/platform/manifest -b android-9.0.0_r44
repo sync -c -j8
```

## 设置环境并选择设备

加载 AOSP 环境配置并选择目标设备：

```shell
source build/envsetup.sh
lunch
lunch aosp_blueline-userdebug
lunch msm8953_64-userdebug
```

## 开始编译 AOSP

使用 `make` 命令编译 AOSP：

```shell
make -j32
```

## 生成 OTA 更新包

如果你需要生成 OTA 更新包，可以使用以下命令：

```shell
mkdir dist_output
make dist DIST_DIR=dist_output
```

## 刷入设备

编译完成后，可以使用以下命令通过 `fastboot` 将系统刷入设备：

```shell
export ANDROID_PRODUCT_OUT=/home/ubuntu/AOSP/out/target/product/blueline
adb reboot bootloader
fastboot flashall -w
```

## 清理与重建

如果需要清理构建过程中的文件，可以使用以下命令：

```shell
make installclean
make clean
make clobber
make
```

## 在模拟器上运行 AOSP

如果你希望在 Android 模拟器上运行 AOSP，可以使用以下命令：

```shell
source build/envsetup.sh
lunch sdk_phone_x86_64-userdebug
emulator
```

以上步骤完成后，模拟器将启动并运行你编译的 AOSP 系统。