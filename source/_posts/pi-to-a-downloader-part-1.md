---
title: 把树莓派改造成挂机下载器（一）
date: 2018-02-14 23:49:11
category: RaspberryPi
tags: 
  - RaspberryPi
  - Aria2
  - Linux 
---

树莓派功能全面，接口丰富，用充电宝即可供电，可将其改造成一个挂机下载器，节省电能，且能够。 ---？

本篇首先实现一个能在局域网内控制的树莓派下载器，下载工具使用aria2，管理端采用WebUI。

# 编译生成Aria2

打开[Github Aria2页面](https://github.com/aria2/aria2/releases)，从latest-release下载tar包，以aria2-1.33.1.tar.gz为例。

```bash
$ wget https://github.com/aria2/aria2/releases/download/release-1.33.1/aria2-1.33.1.tar.gz
$ tar -xf aria2-1.33.1.tar.gz
```

安装必要的依赖包：
```bash
 $ sudo apt install libssl-dev libssh2-1-dev libc-ares-dev libexpat1-dev zlib1g-dev libsqlite3-dev pkg-config
```
为了方便起见，生成目录/home/pi/aira2

```bas
$ cd /home/pi/aria2
$ /path/to/aria2/source/code/configure
$ make
```

在树莓派上编译aria2耗时相当长，可以坐和放宽。经过漫长的等待，在`/home/pi/aria2/src`中生成了编译好的aria2c。可以运行`./aria2c`，查看终端是否有类似的输出：

```bash
Specify at least one URL.
Usage: aria2c [OPTIONS] [URI | MAGNET | TORRENT_FILE | METALINK_FILE]...
See 'aria2c -h'.
```

这样，Aria2就算编译完成了。

# 以后台模式运行Aria2

由于Aria2本身没有提供daemon运行模式，因此需要借助screen来实现Aria2在后台运行。安装screen`$ sudo apt install screen`,

```bash
$ screen -dmS aria2 /home/pi/aria2/src/aria2c --enable-rpc --continue=true --rpc-listen-all=true --rpc-allow-origin-all=true
```

aria2的rpc设置项比较多，可以键入`/path/to/aira2/src/aria2c --help=#rpc`来查看所有相关的设置项。

# 安装网页端控制台

直接输入命令去控制Aria2显然是不现实的，因此要借助Aria2提供的控制端口，配合网页控制台来使用Aria2。可供Aria2使用的网页端工具有很多，这里选择[Aria2NG](https://github.com/mayswind/AriaNg)，在release下在最新版本，解压到树莓派上，例如，把文件解压到`/home/pi/aria2-ng`目录，可用命令`unzip `。

接下来，就是配置一个http服务来显示这个控制台网页，这里采用`nginx`作为服务器。在终端输入

`$ sudo apt install nginx`来安装nginx。然后，进入nginx的配置文件夹`/etc/nginx/conf.d`，将以下内容保存为`aria2ng.conf`：

```nginx
server {
    listen 1600;
    
    location / {
        root /home/pi/aria2-ng;
        index index.html index.htm;
    } 
}
```



