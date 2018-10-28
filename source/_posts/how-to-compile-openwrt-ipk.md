---
title: 「OpenWrt」之编译小记
date: 2017-09-28 00:38:00
thumbnail: https://i.loli.net/2018/10/28/5bd5a14c3c642.png
tags:
- openwrt
---
最近课堂上的专业课不好玩 ，于是入了一个破二手路由器 `TP-LINK WR742N` 折腾着玩 ，以下简称 `WR742` 。 ~~后来又入了斐讯K2路由器来折腾。~~
这是一款上市很久的型号了，官方固件都出到v6了，好在和 `WR740` 硬件构造上基本一样，可以直接使用 openwrt 提供的 `WR740` 的固件 , 不过光有固件不折腾折腾怎么行呢？  

<!-- more -->

### 预期目的

- 自动通过 H3C 校园网 802.1x 认证 ✔
- 自动科学上网 ✔
- 免流上校园网 ✖

Openwrt 官方提供编译用 SDK，无需下载整个源码即可快速编译 ipk 包。所以我只要下载 SDK 后配置好再编译就可以了。
来，编译几个 ipk 包试试。逛了一圈 Github ，找到能够满足 H3C 认证要求的项目 `njit8021xclient` , 满足科学上网的项目 `openwrt-ssr`。

### 小坑小记

- 环境内存尽可能大（采用2G内存)
- 内存无法满足需求可开启 swap 空间
- 硬盘留下足够的空间（采用30G SSD硬盘)
- 切换到非 root 用户执行编译操作 

为保证环境纯净，就临时租用了一台 US 的主机 ，保证不会出现像国内服务器的404现象。选择按量付费 ，等编译完之后提取固件和 ipk 后留个快照就可以退掉主机了 ，十分节省软妹币和维护时间 ，我才不会说我全程都是在手机操作的呢（笑。

### 0x00 编译环境

- Ubuntu `16.04 xenial`
- OpenWrt SDK `Chaos Calmer 15.05.1`
- Platform `ar71xx`
- Device `TP-LINK WR742N Magic Version`

### 0x01 安装依赖包

```bash
sudo apt-get update
sudo apt-get install git-core build-essential libssl-dev libncurses5-dev unzip gawk zlib1g-dev 
```

### 0x02 准备OpenWrt SDK

```bash       
cd ~                                                                     
wget https://downloads.openwrt.org/chaos_calmer/15.05.1/ar71xx/generic/OpenWrt-SDK-15.05.1-ar71xx-generic_gcc-4.8-linaro_uClibc-0.9.33.2.Linux-x86_64.tar.bz2
tar xjf OpenWrt-SDK-ar71xx-*
mv OpenWrt-SDK-ar71xx-* openwrt
cd openwrt
cp feeds.conf.default feeds.conf
```

### 0x03 编译前配置 

```bash
# 安装 feeds
./scripts/feeds update packages
./scripts/feeds install libpcre
# 获取 Makefile
git clone https://github.com/ywb94/openwrt-ssr.git package/openwrt-ssr
make menuconfig
# 选择要编译的包 
#luci -> 3. Applications -> luci-app-shadowsocksR         原始版本
#luci -> 3. Applications -> luci-app-shadowsocksR-GFW     GFWList版本
#Network -> njit8021xclient
#Network -> njit8021xclient-web
```

### 0x04 开始编译

`make package/openwrt-ssr/compile v=99`
编译成功后的文件在 `~/openwrt/bin/target/ar71xx` 目录下。

### 已知bug

网口顺序是反着来的，即硬件上的LAN1口=软件上的WAN口，不过不影响使用，那我也反着插好了🌚。  

### 参考资料

- [官方 Wiki](https://wiki.openwrt.org/doc/howto/buildroot.exigence)
- [官方 Development](https://wiki.openwrt.org/doc/faq/development)
- [njit8021xclient](https://github.com/liuqun/njit8021xclient)
- [openwrt-ssr](https://github.com/ywb94/openwrt-ssr)
