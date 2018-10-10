---
title: ubuntu18.04中安装iNode
toc: false
date: 2018-09-01 17:52:20
categories:
- methods
tags:
- ubuntu
- iNode
---





首先在学校官网下载32位版本的iNode包（64位一直无法安装成功因此选择安装32位版本的）。

<!-- more -->

解压。



安装各种依赖库（如果某个命令无法运行可以在安装目录下运行`./iNodeClient.sh`查看当前需要安装的依赖）：

```powershell
sudo apt-get install lib32ncurses5
sudo apt-get install lib32z1
sudo aptitude install libgtk-x11-2.0
sudo apt-get install libgtk2.0-0:i386 libnss3:i386 libcurl3-gnutls:i386 libidn11:i386 libpango1.0-0:i386 libpangox-1.0-0:i386 libpangoxft-1.0-0:i386
sudo apt-get install libgtk2.0-0:i386 libxxf86vm1:i386 libsm6:i386 lib32stdc++6
sudo apt-get dist-upgrade
sudo ln -s /usr/lib/i386-linux-gnu/libjpeg.so.8 /usr/lib/i386-linux-gnu/libjpeg.so.62
sudo ln -s /usr/lib/i386-linux-gnu/libtiff.so.5 /usr/lib/i386-linux-gnu/libtiff.so.3
sudo apt-get install murrine-themes
sudo apt-get install gtk2-engines-murrine
sudo apt-get install libgtkmm-2.4-dev
sudo apt-get install libcanberra-gtk-module:i386
```



在解压后的目录中运行：

```powershell
sudo ./install.sh
```



这个时候`iNodeClient.destop`就可以点击运行了。



添加用户名和密码后将`NIC`修改为`enp7s0`即可成功连接。



[参考链接](https://blog.csdn.net/a845717607/article/details/52563005)