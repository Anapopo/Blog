---
title: 记录 Android 的编译过程
date: 2017-07-19 13:14:56
tags:
  - Android
  - 编程
---
#### 0x00 前期准备工作
+ Ubuntu系统，内存至少8GB，硬盘至少预留100GB空间
+ 自备梯子用于下载安卓源码或者直接前往清华大学镜像源下载

<!-- more -->

#### 0x01 安装Android SDK平台工具
输入`adb`或者`fastboot`，如果提示不能使用，那就需要[从谷歌下载](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)，解压后并添加进环境变量当中

#### 0x02 安装编译所需的依赖包
这里将使用到`apt-get`命令，执行前执行一次`apt-get update && apt-get upgrade`用于更新依赖包列表
```bash    
$ sudo apt-get install bc bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf \
imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev libesd0-dev liblz4-tool libncurses5-dev \
libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool \
squashfs-tools xsltproc zip zlib1g-dev
```
对于Ubuntu系统版本低于`16.04`的请把依赖包中的`libwxgtk3.0-dev`更换为`libwxgtk2.8-dev`
Java版本则推荐安装`openjdk-8-jdk`这个依赖包

#### 0x03 创建源码安装目录
```bash
$ mkdir ~/lineageos
$ mkdir ~/bin
```
#### 0x04 安装`repo`命令
你可能借助梯子[从谷歌下载](https://storage.googleapis.com/git-repo-downloads/repo)并赋予其可执行权限
```bash
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
```
添加入repo命令所在的bin文件夹至环境变量中
这样你就可以在任何地方执行repo命令

#### 0x05 开始拉取Android源码
拉取之前需要做一步准备工作，设置`Git`的邮箱和用户名
```bash
$ git config --global user.email "Your Email"
$ git config --global user.name "Your Name"
```
执行以下指令开始拉取，然后去喝杯咖啡，休息一下
```bash   
$ cd ~/lineagos
$ repo init -u https://github.com/LineageOS/android.git -b cm-14.1
$ repo sync
```

#### 0x06 准备你自己的设备文件
参照如下
+ [Andorid_device_samsung_a7xltechn](https://github.com/Anapopo/android_device_samsung_a7xltechn)
+ [Android_vendor_samsung_a7xltechn](https://github.com/Anapopo/android_vendor_samsung_a7xltechn)
+ [Android_kernel_samsung_a7xltechn](https://github.com/Anapopo/android_kernel_samsung_a7xltechn)

#### 0x07 配置虚拟内存Swap
```bash
$ dd if=/dev/zero of=/swap1 bs=1M count=4096
$ mkswap /swap1
$ swapon /swap1
$ chmod 0600 /swap1
$ vim /etc/fstab # add "/swap1 swap swap defaults 0 0" in the last row
```

#### 0x08 配置ccache用于加快编译速度
```bash
$ export USE_CCACHE=1
$ cd ~/lineaegos
$ prebuilts/misc/linux-x86/ccache/ccache -M 50G
```

#### 0x09 配置Jack编译器
Jack是一款新的Java编译器，此处配置是为了给Jack编译时分配足够的内存，以免因为内存不足而编译失败
```bash    
$ export ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4G"
```

#### 0x10 开始编译吧
```bash    
$ cd ~/lineageos
$ export WITH_SU=true #开启root权限（可选） 
$ source ./build/envsetup.sh #载入环境
$ brunch <Your Device Name>
```
这又是一段漫长的等待...
一旦当编译完成后就可以进入输出目录去找你的刷机包了
当然编译中也会不可避免地出现一些报错，这就需要自己去Google或者XDA找寻答案了

#### 0x99 参考资料
+ [LineageOS Wiki](https://wiki.lineageos.org/devices/kltechn/build)
