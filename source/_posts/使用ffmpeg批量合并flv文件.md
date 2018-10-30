---
title: 使用ffmpeg批量合并flv文件
toc: false
date: 2018-10-14 16:08:19
categories:
- methods
tags:
- ffmpeg
- flv
---

使用`youtube-dl`从b站下的视频是分part的，因此需要合并一下。

<!-- more -->

首先需要把那些分part的视频重命名一下

- 去掉中文和特殊字符
- 数字前位用0补全，如01~19

然后在终端中进入当前目录执行命令：

```shell
for f in *.flv; do echo "file '$f'" >> mylist.txt; done
ffmpeg -f concat -i mylist.txt -c copy output.flv
```

