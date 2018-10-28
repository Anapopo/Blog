---
title: 修复 Arch Linux 下 Typora 段错误
date: 2018-08-12 19:30:42
thumbnail:
tags:
- archlinux
- typora
---
### 段错误问题

你是一名资深的 Arch Linux 用户，你喜欢经常更新系统。
而且你还是一名热爱写作的 Typora 用户。
那么问题产生了。

<!--more-->

在最近一次更新系统后，启动 Typora 时会报 {% ruby 段错误| Segmentation Fault %}。
经过一番查阅 [Issues](https://github.com/typora/typora-issues/issues/1671) 后，发现报错原因在于 `glibc` 。
各位可以查看一下版本号 `2.28-4-x86_64 `
于是使用降级工具将 `glibc` 降级到 `2.27-3-x86_64 `即可，等待新版本修复吧。
困扰了多天的问题终于解决了。

```bash
yay -S downgrade
downgrade glibc
# 选择版本 2.27-3-x86_64
```

### 方案来源

+ [Typora Issues](https://github.com/typora/typora-issues/issues/1671)
+ [glibc 2.28 cause core dump on electron based apps](https://bugs.archlinux.org/task/59550)

### 后记
我又回来了，因为降级了版本后，依赖于 `GLIBC_2.28` 的 `libQt5Core.so` 无法运行，导致 `sddm` 也无法运行，所以我连桌面环境也进不去了。
于是我又把 `glibc` 的版本刷回来了 QAQ, 现在只能等 glibc 修复关于 `electron` 应用的 bug 了。
