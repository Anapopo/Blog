---
title: Ubuntu 安装 Caddy 为系统级服务
date: 2018-07-26 11:30:00
tags:
- caddy
- systemd
---
### 引言

关于Caddy，其作为开箱即用的 HTTPS 服务器而闻名，在此便不多赘述。我根据 Caddy 官方提供的英文文档，一步一步地将 Caddy 通过`linux-systemd`安装至 Ubuntu 系统中，该操作方法也适用于其他将`linux-systemd`作为启动管理工具的 Linux 发行版，包括 ArchLinux。

<!-- more -->

### 下载 tar.gz 压缩包

首先打开 Caddy 官方的[下载页面](https://caddyserver.com/download)，选择 Linux 平台，添加插件并选择 {% ruby 个人协议|Personal License %}，点击开始下载。

经过龟速的下载后得到 `caddy_v0.XX.0_linux_amd64_personal.tar.gz`，然后 `tar zxvf` 解压之，记得加参数 `-C` 解压到文件夹里呢。

### 放置 Caddy 的二进制文件

拷贝压缩包中附带的 Caddy 二进制文件到 `/usr/local/bin` 并更改权限。
```bash
sudo cp /path/to/caddy /usr/local/bin
sudo chown root:root /usr/local/bin/caddy
sudo chmod 755 /usr/local/bin/caddy
```

允许 Caddy 的二进制文件绑定端口（例如80,443）
```bash
sudo setcap 'cap_net_bind_service=+ep' /usr/local/bin/caddy
```

### 构建 Web 服务资源

建立所需要的用户组、用户和文件夹，若组ID 33 被占用，可使用其他ID。

```bash
sudo groupadd -g 33 www-data
sudo useradd \
  -g www-data --no-user-group \
  --home-dir /var/www --no-create-home \
  --shell /usr/sbin/nologin \
  --system --uid 33 www-data

sudo mkdir /etc/caddy
sudo chown -R root:www-data /etc/caddy
sudo mkdir /etc/ssl/caddy
sudo chown -R root:www-data /etc/ssl/caddy
sudo chmod 0770 /etc/ssl/caddy
```

正确放置 `Caddyfile` 文件并设置权限
```bash
sudo cp /path/to/Caddyfile /etc/caddy/
sudo chown www-data:www-data /etc/caddy/Caddyfile
sudo chmod 444 /etc/caddy/Caddyfile
```

创建网站根目录文件夹
```bash
sudo mkdir /var/www
sudo chown www-data:www-data /var/www
sudo chmod 555 /var/www
```

添加如下配置至 `Caddyfile`
```Caddyfile
example.com {
    root /var/www/example.com
    ...
}
```

### 配置 Systemd 服务单元

安装 `systemd` 服务单元，重启  `systemd` 并启动 Caddy

```bash
wget https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service
sudo cp caddy.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/caddy.service
sudo chmod 644 /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl start caddy.service
```

允许 Caddy 服务开机启动
```bash
sudo systemctl enable caddy.service
```

若 Caddy 没有成功启动，则可以查看日志来排查引起错误的原因。
```bash
journalctl --boot -u caddy.service
```

### 总结
最后往系统中添加的文件如下
```
/usr/local/bin/caddy
/etc/systemd/system/caddy.service
/etc/ssl/caddy
```

For Debug
```
ls -al $(which caddy)
pacman -Ql caddy-full-bin # For Arch Linux
journalctl -xeu caddy --priority 5 --no-pager --no-hostname
systemctl cat caddy
```

+ Caddy 可以通过 `systemctl` 进行重启、自启管理等操作
+ Caddy 的配置文件位于 `/etc/caddy/Caddyfile`
+ Caddy 的网站目录位于 `/var/www`
+ 可通过 `systemctl status caddy`查看 Caddy 的运行状态

### 鸣谢

[Caddy 官方英文教程](https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/README.md)

