---
title: ubuntu 18.04下安装稳定版Chrome谷歌浏览器
toc: false
date: 2018-09-02 08:30:57
categories:
- methods
tags:
- ubuntu
- Chrome
---



由于官方下载页面打不开，因此找了一个网盘下载的，[网盘下载点击这里](https://pan.baidu.com/s/1vC974ES6Y4UPdyRNTAJpZg)。

<!-- more -->

安装相关依赖：

```powershell
sudo apt-get install libcurl3
sudo apt-get install libappindicator1
sudo apt-get install libpango1.0-0
sudo apt-get install libgooglepinyin0
```



进入下载目录，运行下列命令即可：

```powershell
sudo mv google-chrome-stable_current_amd64.deb /usr/local
cd /usr/local
sudo dpkg -i google-chrome-stable_current_amd64.deb
```



进行更新：

```powershell
sudo apt-get update
sudo apt-get upgrade
```





显示如下提示即安装成功：

```powershell
zmj@zmj:/usr/local$ sudo dpkg -i google-chrome-stable_current_amd64.deb
Selecting previously unselected package google-chrome-stable.
(Reading database ... 217976 files and directories currently installed.)
Preparing to unpack google-chrome-stable_current_amd64.deb ...
Unpacking google-chrome-stable (51.0.2704.106-1) ...
Setting up google-chrome-stable (51.0.2704.106-1) ...
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/x-www-browser (x-www-browser) in auto mode
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/gnome-www-browser (gnome-www-browser) in auto mode
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/google-chrome (google-chrome) in auto mode
Processing triggers for gnome-menus (3.13.3-11ubuntu1.1) ...
Processing triggers for desktop-file-utils (0.23-1ubuntu3.18.04.1) ...
Processing triggers for mime-support (3.60ubuntu1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...

```

