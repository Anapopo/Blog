---
title: 全新安装 Ubuntu 18.04
date: 2018-09-19 16:51:00
thumbnail: https://i.loli.net/2018/10/28/5bd59ce53dafa.png
tags:
 - linux
---
首先打开[「清华大学镜像站」](https://mirrors4.tuna.tsinghua.edu.cn/help/ubuntu/)，获取 Ubuntu 的国内镜像地址。不要问我为什么用清华镜像源，因为主页看起来很~~可口~~漂亮，速度比较快。

<!-- more -->

然后编辑 `/etc/apt/sources.list` 用以下内容替换原有内容。

![](https://i.loli.net/2018/09/19/5ba1ed9c35a39.png)

### 0、安装基本组件

```bash
sudo apt update && sudo apt upgrade
sudo apt install git curl vim zsh fcitx gnome-session gnome-tweak-tool
```

然后是配置 ZSH 的一系列组件，这里先附上三个项目地址，下面会~~用到~~看到它们的名字：

+ [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
+ [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
+ [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

当前状态是刚装完 `zsh`，什么都还没做，如果当前默认 Shell 不是 `zsh` 的话，可以通过 `chsh` 命令来指定当前用户的默认 Shell。

依次执行官方提供的三个脚本：

```bash
# 这个安装 oh-my-zsh 到你的 `~/.oh-my-zsh` 目录
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# 这个安装命令自动补全插件
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 这个安装语法自动高亮插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

等待安装完毕以后，打开你的 `.zshrc` 文件，修改如下：

```bash
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting # 这个一定要在最后一行
)
```

最后重新登录即可生效，不过我一般都是 `source .zshrc` 直接生效。

### 1、配置搜狗输入法

首先确保 `fcitx`安装成功，可以先执行 `fcitx-diagnose` 查看下当前的状态。
然后前往搜狗输入法官网 [戳我](https://pinyin.sogou.com/linux/) 下载 deb 包，最后双击安装。

![](https://i.loli.net/2018/09/19/5ba1eec212e57.png)

接下来是配置部分：
(1)、在普通的使用 `Xorg` 协议的 Gnome 上

```bash
# 编辑 ~/.xprofile 没有就创建一个
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
# fcitx 的自启动
fcitx &
```

(2)、在使用了 `Wayland` 协议的 Gnome 上

```bash
# 由于 Wayland 协议无法读取 `.xprofile` 文件，所以编辑 `/etc/environment` 文件
# 原来的此处应该有定义 PATH 变量，不要去改变它。
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

关于 `fcitx` 的自启动，把 `/usr/share/fcitx/xdg/autostart/fcitx-autostart.desktop` 文件拷贝至 `/etc/xdg/autostart`下即可。

[*可选项] 由于使用了 `fcitx`，具有强迫症的话可以执行 `sudo apt purge ibus` 把自带的 `ibus`卸载。
[*可选项] 可以使用搜狗输入法的所有皮肤（除动态皮肤以外），所以去官网挑一个喜欢的皮肤下载后双击安装即可。推荐 [柔兰](https://pinyin.sogou.com/skins/detail/view/info/559973)

### 2、安装常用软件

有很多软件都是跨平台的，跨平台的技术大概有 `Electron`、`Qt`、`AppImage`等等。
需要什么软件，只要通过 `<软件名> + Linux` 关键词去搜索引擎找一找就可以了。

我安装了 `Visual Studio Code`、`Telegram`、`Typora` 这三款。
其中 `VSCode`可以在官网下载到 deb 包，而在 Ubuntu 官方软件仓库中的`Telegram` 采用了 `snap` 技术。我尝试安装了 N 次都失败了，所以去官网下载了二进制包，写个 `DesktopEntry` 丢进 `~/.local/share/applications/` 就直接拿来用了。

`DesktopEntry` 一并附上，来源 [@METO](https://i-meto.com/telegram-ibus/)，在此感谢~
文件名 ：`telegramdesktop.desktop`

```
[Desktop Entry]
Version=1.0
Name=Telegram Desktop
Comment=Official desktop application for the Telegram messaging service
TryExec=/opt/Telegram/Telegram
Exec=/opt/Telegram/Telegram -- %u
Icon=telegram
Terminal=false
StartupWMClass=TelegramDesktop
Type=Application
Categories=Network;InstantMessaging;Qt;
MimeType=x-scheme-handler/tg;
X-Desktop-File-Install-Version=0.23
```

`Typora`是一款很棒的 MarkDown 写作软件，安装步骤也比较简单，只要添加软件源后 `sudo apt install typora` 就好了。

### 3、配置科学上网环境

#### 其一（electron-ssr）：

在 Ubuntu下较好的科学上网方式除了普通的 `ShadowsocksR-libev`、 `Shadowsocks-Qt` 和 `Python` 方式，还可以采用 `electron-ssr`这个项目，在 Release 界面有提供 deb 包，甚至还有 ArchLinux 的 pacman 包。

界面如下图：
![](https://i.loli.net/2018/09/19/5ba1f614e89a6.png)

由于 `electron-ssr` 采用 Python 作为后端，所以在启动 `electron-ssr` 之前，要确保 Python 环境配置好。如果系统中没有 `python` 命令的话，程序是可以打开，但是没有任何报错（终端启动可以看到报错），使用 `netstat -lntp` 查看也没有 `1080` 端口被打开。

首先查看 `python` 命令是否存在，不存在就先执行 `sudo ln /usr/bin/python3 /usr/bin/python`，这里我用一个软链接把 `python3` 链接到 `python` 上，这样的话系统查找 `python` 的时候就直接可以使用 `python3` 。

打开 `electron-ssr` 选择不启用代理后，可以 使用 `netstat -lntp` 查看到 1080 端口已经打开，这是一种 `socks5` 代理，代理地址为 `127.0.0.1:1080`。

[巧合？] 我在测试启动的时候提示找不到 `canberra-gtk-module`，遂安装 `libcanberra-gtk-module`解决。
[小提示] 使用 netstat 命令之前需要先安装 `net-tools` 软件包。
[*可选项] 搭配浏览器拓展 `SwitchyOmega` 可以获得更加精准的流量转发。

#### 其二（proxychains-ng）：

这种方案更加适用于终端，首先准备编译环境 `sudo apt install build-essential`
打开[项目主页](https://github.com/rofl0r/proxychains-ng)，把源码通过 `git clone` 或是直接下载压缩包至本地并解压。

```bash
cd proxychains-ng
./configure --prefix=/usr --sysconfdir=/etc
sudo make && sudo install && sudo install-config
sudo make clean
```

执行完毕后可以发现库文件、二进制文件和配置文件都已经各就各位。这，就是 Unix 哲学。

+ /usr/lib/libproxychains4.so
+ /usr/bin/proxychains4
+ /etc/proxychains.conf

接着修改配置文件 `sudo vim /etc/proxychains.conf `（忘记 sudo 赶紧退出来，别编辑了），来到最后一行。

```bash
# 可以使用多种代理，程序会逐行尝试，改成下面的代理地址并保存
[ProxyList]
socks5 127.0.0.1 1080
```

现在可以通过 `proxychains4 <需要代理的命令>` 来使用代理，举个例子 `proxychains4 curl https://www.google.com`，不出意外可以看到一堆文字。

Q: 那么老师能不能给力一点啊？
A: 再懒一点的话可把 `alias ssr='proxychains4'`写入你的  .bashrc 或是 .zshrc 文件里，使用 `ssr curl https://www.google.com`

Q: 那么老师能不能再给力一点啊？
A: 使用 `proxychains4 -q /bin/zsh` 命令绑定一个 Shell，这样在新打开的 Shell 里面执行任何命令都是通过代理的。

Q: 那么老师......
A: 好了好了，你去吧。

#### 其三（ShadowsocksR-libev）:

### 4、配置 GPG 和 SSH

首先导入 GPG 公钥和私钥，然后设为绝对信任（什么？连自己的公钥都不设绝对信任是人吗？）

```bash
gpg --import <path/to/your/public_gpg_key> <path/to/your/secret_gpg_key>
# 接下来绝对信任自己的公钥
gpg --edit-key <公钥ID>
gpg> trust
# 选择绝对信任
gpg> quit
```

创建 `~/.ssh` 文件夹，把自己的 SSH 私钥丢进去，通通 `chmod 600` 确保安全（不执行的话，连服务器会被拒）
然后创建 `config` 文件，按照下面的格式写入配置

```bash
HostName b # 配置完毕就可以 使用 `ssh b` 连接主机了
Host ssh.baidu.com # 你的主机地址
User li # 你的主机用户名
Port 22 # 一般是22，不写默认也是22
IdentityFile <path/to/your/ssh_secret_key> # 指向你的私钥

# 再来一个 Github 的
Host github.com
User git
IdentityFile ~/.ssh/Git_Anapopo
```

最后是配置 git，由于直接敲指令太麻烦，所以我一般都是事先备份好 `dotfiles`，然后安装的时候直接软链接，
这里直接对 Commits 开启了 GPG 签名，以确保 Commits 确实是我本人提交的（虽然感觉没有人会冒充我，还给我提交Commits QAQ）。

```bash
# 创建 `~/.gitconfig` 文件
[user]
    email = <your_email>
    name = <your_name>
    signingkey = <your_short_key_id>
[commit]
    gpgsign = true
```

### 5、配置主题环境

首先创建用户本地主题文件夹 `~/.themes`、`~/.icons`，然后打开 Gnome-Look [官网](https://www.gnome-look.org/browse/cat/135/ord/latest/)，挑选你喜欢的主题、图标、光标等，把它下载下来解压。主题**文件夹**丢进 `~/.themes`，光标和图标**文件夹**丢进 `~/.icons`里面。

![](https://i.loli.net/2018/09/19/5ba20495190fe.png)

然后运行 `gnome-tweaks` 命令开主题设置，选择需要的主题。

![](https://i.loli.net/2018/09/19/5ba2058c86c1e.png)

最后，大功告成，享受 Ubuntu 带来的快乐吧~（重装了两次，双份的快乐。）

![](https://i.loli.net/2018/09/19/5ba2071ee106e.jpg)
