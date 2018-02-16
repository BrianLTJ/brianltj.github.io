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
为了方便起见，使用静态编译的aria2，生成目录/home/pi/aira2

```bas
$ cd /home/pi/aria2
$ /path/to/aria2/source/code/configure ARIA2_STATIC=yes
$ make
```

在树莓派上编译aria2耗时相当长，可以坐和放宽。