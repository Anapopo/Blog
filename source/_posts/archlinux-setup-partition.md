---
title: 「Arch Linux」配置之磁盘分区
date: 2018-04-11 21:24:01
tags:
- archlinux
---
这是一篇 「GNU/Linux」 新手的 「Arch Linux」 磁盘分区完全指南。

<!-- more -->

> 下载镜像和虚拟机加载部分另开一篇再讲，这篇着重于磁盘分区时遇到的坑。

#### Startup

分区前先看看磁盘状态`lsblk -f`  
```plaintext
NAME      FSTYPE     LABEL        UUID                     MOUNTPOINT
loop0     squashfs                                         /run/archiso/sfs/airootfs
sda
sr0       iso9660    ARCH_201804  2018-04-01-05-06-08-00   /run/archiso/bootmnt
...
```

执行 `fdisk /dev/sda` 后输入 `m` 查看帮助  
```plaintext
Help:
  DOS (MBR)
   a   toggle a bootable flag
   b   edit nested BSD disklabel
   c   toggle the dos compatibility flag

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   u   change display/entry units
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table
```

列出几个常用的命令：
* **g** 初始化 GPT 分区表
* **n** 添加新分区
* **d** 删除分区
* **t** 修改分区类型


#### Partition For BIOS

1. 初始化 GPT 磁盘分区表

```bash
Command (m for help): g
Created a new GPT disklabel (GUID: ECE79DD4-88DF-4D4D-82F2-D665816100AD)
```

2. 建立 `BIOS boot` 分区

```bash
Command (m for help): n
Partition number (1-128, default 1): 1
First sector (2048-16777182, default 2048): 2048
Last sector, +sector or +size{K,M,G,T,P} (2048-16777215, default 16777215): +1M

Created a new partition 1 of type 'Linux filesystem' and of size 1 MiB.

Command (m for help): t
Selected partition 1
Partition type (type L to list all types): 4
Changed typeof partition 'Linux filesystem' to 'BIOS boot'.
```

3. 建立 `SWAP` 分区

```bash
Command (m for help): n
Partition number (2-128, default 2): 2
First sector (4096-16777182, default 4096): 4096
Last sector, +sector or +size{K,M,G,T,P} (4096-16777215, default 16777215): +2G

Created a new partition 2 of type 'Linux filesystem' and of size 2 GiB.
```

4. 建立 `Linux` 文件系统

```bash
Command (m for help): n
Partition number (3-128, default 3): 3
First sector (4198400-16777182, default 4198400): 4198400
Last sector, +sector or +size{K,M,G,T,P} (4198400-16777215, default 16777215):

Created a new partition 3 of type 'Linux filesystem' and of size 6 GiB.
```

5. 写入分区表

```bash
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

6. 格式化磁盘

```bash
# 先查看目前分区表
# fdisk -l
...
Device      Start       End    Sectors    Size    Type
/dev/sda1    2048      4095       2048      1M    BIOS boot
/dev/sda2    4096   4198399    4194304      2G    Linux filesystem
/dev/sda3 4198400  16777182   12578783      6G    Linux filesystem
...

# 格式化磁盘
# mkfs.ext4 /dev/sda3
...省略输出...

# 建立Swap区
# mkswap /dev/sda2
...
# swapon /dev/sda2
...

# 查看分区表
# lsblk -f
NAME      FSTYPE       LABEL   UUID   MOUNTPOINT
...
sda
├── sda1 
├── sda2   swap
├── sda3   ext4  ARCH_201804
...
```
#### Partition For UEFI

类似地分区如下
```shell
Name         Size    Type
/dev/sda1    512M    EFI System
/dev/sda2    2G      SWAP
/dev/sda3    118G    Linux File System
```

#### Why

这里建立 `BIOS boot` 分区是为了后面 `grub` 的安装。~~我才不会说我因为这个重新安装了三遍呢~~  
根据官方 Wiki 的说明：
> 安装 GRUB 前，在一个没有文件系统的磁盘上，用 fdisk 或 gdisk 创建一个 +1M 分区，设置为 BIOS boot 类型，在 fdisk 中的类型号是 4, 在 gdisk 中的类型是 ef02，在 parted 中是 bios_grub。此分区可以在磁盘前 2TB 的任何位置。分区建立好后，按下面的命令安装启动管理器。

如果没有 `BIOS boot` 分区，在执行 `grub-install` 时将会报错QAQ

```bash
# grub-install --target=i386-pc /dev/sda
```

#### Reference

[给 GNU/Linux 萌新的 Arch Linux 安装指南](https://blog.yoitsu.moe/arch-linux/installing_arch_linux_for_complete_newbies.html)
