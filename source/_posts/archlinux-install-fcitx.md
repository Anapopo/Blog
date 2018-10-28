---
title: 为 Arch Linux 安装搜狗输入法
date: 2018-05-10 18:23:10
thumbnail: https://i.loli.net/2018/05/10/5af418e08c961.png
tags:
- archlinux
---
折腾过「Ibus」，受不了「Ibus」丑陋的界面，最后还是滚回了「Fcitx」，因为皮肤实在是太好看了~\(≧▽≦)/~啦啦啦。

<!-- more -->

### 安装过程

+ 直接从官方仓库 [archlinuxcn] 安装

```bash
yaourt -S fcitx fcitx-im fcitx-sogoupinyin kcm-fcitx
```

+ 配置输入法，编辑 `~/.xprofile`，加入如下内容：

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
fcitx -d -r --enable sogou-qimpanel
```

这样配置的话，同时能在GTK界面和QT界面中使用输入法。

+ 接着从 [AUR] 安装我打包的「柔兰」皮肤

```bash
yaourt -S sogoupinyin-skin-roulan
```

安装后直接在搜狗输入法设置里选择皮肤「roulan」就好了，请享用~

[皮肤来源](https://pinyin.sogou.com/skins/detail/view/info/559973) [AUR地址](https://aur.archlinux.org/packages/sogoupinyin-skin-roulan/)
