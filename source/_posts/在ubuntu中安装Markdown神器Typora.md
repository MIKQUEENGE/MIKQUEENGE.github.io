---
title: 在ubuntu中安装Markdown神器Typora
toc: false
date: 2018-09-01 17:48:15
categories:
- methods
tags:
- ubuntu
- Typora
- Markdown
---



在终端中执行以下命令即可：

```powershell
# optional, but recommended

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

# add Typora's repository

sudo add-apt-repository 'deb http://typora.io linux/'

sudo apt-get update

# install typora

sudo apt-get install typora
```



[参考链接](https://blog.csdn.net/caoyangyang123/article/details/79484697)