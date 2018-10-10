---
title: Ubuntu下使用Deepin-wine的移植版安装qq微信等
toc: false
date: 2018-09-18 16:12:49
categories:
- methods
tags:
- ubuntu
---

下载Deepin-wine的Ubuntu移植版：

`git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git`

进入 `deepin-wine-for-ubuntu/`文件夹

在终端内运行`./install.sh`

<!-- more -->

这样就安装完成啦，现在就可以安装qq微信之类的软件啦：

[所有deepin-wine内支持的windows软件下载地址](http://mirrors.aliyun.com/deepin/pool/non-free/d/)

其中qq下载地址为：http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im/deepin.com.qq.im_8.9.19983deepin23_i386.deb

微信下载地址为：http://202.116.81.74/cache/6/01/mirrors.aliyun.com/2fcea7de3a93db0339ae7eb601f36a83/deepin.com.wechat_2.6.2.31deepin0_i386.deb

下载之后进入下载目录，终端内运行：

`sudo dpkg -i deepin.com.qq.im_8.9.19983deepin23_i386.deb `

和

`sudo dpkg -i deepin.com.wechat_2.6.2.31deepin0_i386.deb`即可。

然后在自己的应用程序目录就可以看到qq和微信了。