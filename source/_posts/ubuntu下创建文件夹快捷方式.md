---
title: ubuntu下创建文件夹快捷方式
toc: false
date: 2018-09-01 17:22:28
categories:
- methods
tags:
- ubuntu
- 快捷方式
---



```powershell
sudo ln -sT [srcDir] [dstDir/name]
```

例如创建`hexo`文件夹的桌面快捷方式：

```powershell
sudo ln -sT '/media/zmj/本地磁盘/hexo' '/home/zmj/Desktop/hexo'
```



[参考链接](https://www.cnblogs.com/tergeldev/p/6336836.html)