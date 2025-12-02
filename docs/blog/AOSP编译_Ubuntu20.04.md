---
title: AOSP编译_Ubuntu20.04
createTime: 2025/12/02 09:25:34
permalink: /blog/38hrmb7t/
---
系统环境

```shell
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.6 LTS
Release:	20.04
Codename:	focal
```

构建指令

```shell
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig openssl libssl-dev

git config --global user.name ubuntu
git config --global user.email ubuntu@ubuntu.com

mkdir AOSP
cd AOSP

sudo apt install python2-minimal
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
sudo update-alternatives --config python
1

mkdir -p ~/.bin
PATH="${HOME}/.bin:${PATH}"
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
chmod a+rx ~/.bin/repo
repo version

repo init -u https://android.googlesource.com/platform/manifest -b android-9.0.0_r44
repo sync -c -j8

source build/envsetup.sh
lunch
lunch aosp_blueline-userdebug
lunch msm8953_64-userdebug
make -j32

# OTA
mkdir dist_output
make dist DIST_DIR=dist_output

export ANDROID_PRODUCT_OUT=/home/ubuntu/AOSP/out/target/product/blueline
adb reboot bootloader
fastboot flashall -w

make installclean
make clean
make clobber
make
```

```bash
source build/envsetup.sh
lunch msm8953_64-userdebug
make installclean
make -j32

```

编译后的产物输出在当前文件夹下的out/target/product目录中。

## 在模拟器上运行
```bash
source build/envsetup.sh
lunch sdk_phone_x86_64-userdebug
emulator
```