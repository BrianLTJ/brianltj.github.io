---
title: set-up-nfs-server-on-pi
date: 2018-02-15 10:42:03
tags:
---



```bash
$ sudo dpkg -l | grep nfs-kernel-server
```

如果没有，则安装nfs-kernel-server

```bash
$ sudo apt install nfs-kernel-server
```



创建NFS专有文件夹，把实际文件夹映射过去

```bash
$ sudo mkdir -p /export/users

$ sudo mount --bind /home/pi/nfstest /export/users
```

编辑配置文件

注意，括号中options之间不能有空格，否则会启动失败。



最后，重启nfs-kernel-server服务。





# Windows下映射NFS

首先在Cortana搜索中键入Windows功能，打开启用或关闭Windows功能，找到 NFS服务这一项，勾选，然后点击确定，等待系统完成配置。

打开Powershell(管理员)，键入

```powershell
PS C:\Windows\system32> .\mount.exe -o anon \\192.168.0.160\export\users Z:
Z: 现已成功连接到 \\192.168.0.160\export\users

命令已成功完成。
```

这条命令将刚刚设置的\export\users目录映射到本地的Z盘，登录方式为匿名登陆。



或者，在资源管理器中，点击 计算机 选项卡，映射网络驱动器，填入地址，如果使用匿名方式，就不要勾选使用其他凭据连接 这一项，然后点击完成，不出意外的话，映射的盘符会出现在 此电脑 中的网络位置这一项，打开后就能和操作本地磁盘一样读写文件了。





参考资料

[SettingUpNFSHowTo](https://help.ubuntu.com/community/SettingUpNFSHowTo)