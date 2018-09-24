---
title: ubuntu18.04开机慢的解决办法
toc: false
date: 2018-09-24 12:49:31
categories:
- methods
tags:
- ubuntu
---

在终端中输入：

```powershell
sudo gedit /etc/default/grub 
```

将打开的文件中的`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`

修改为`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash noresume"`

保存退出后在终端中运行下面的命令来让修改生效：

```powershell
sudo update-grub
```

重启电脑感受一下吧！