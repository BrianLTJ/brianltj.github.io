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

首先在Cortana搜索中键入 Windows功能，打开 启用或关闭Windows功能，找到 NFS服务这一项，勾选，然后点击确定，等待系统完成配置。配置完成后可能需要重启一下，否则mount命令连接完成后虚拟盘符不会出现。

打开命令行，键入

```powershell
PS C:\Windows\system32> .\mount.exe -o anon \\192.168.0.160\export\users Z:
Z: 现已成功连接到 \\192.168.0.160\export\users

命令已成功完成。
```

这条命令将刚刚设置的\export\users目录映射到本地的Z盘，登录方式为匿名登陆。



或者，在资源管理器中，点击 计算机 选项卡，映射网络驱动器，填入地址，如果使用匿名方式，就不要勾选使用其他凭据连接 这一项，然后点击完成，不出意外的话，映射的盘符会出现在 此电脑 中的网络位置这一项，打开后就能和操作本地磁盘一样读写文件了。



# 坑

首先，Windows中提供的mount命令有缺陷，一个最严重的问题就是中文字符乱码，无法新建文件夹（创建文件夹默认名称就是中文），含中文字符的文件、文件夹无法上传，上传文件会提示已有相同文件名等等。mount命令虽然可以指定编码，但utf8不在其支持的范围，所以相当不便。网上也没有太多Windows端的、支持UTF8的NFS Client。

第二，NFS的传输速率似乎不是那么高，在同一内网（无线2.4G Wifi，上传端网卡标称150Mbps）环境下传输速率大约为2~3MB/s，改用有线连接树莓派，上传速率大约有4MB/s，而使用HTTP下载可达6MB/s（初步测试结果，还没达到顶峰就下载完毕了）。





参考资料

[SettingUpNFSHowTo](https://help.ubuntu.com/community/SettingUpNFSHowTo)