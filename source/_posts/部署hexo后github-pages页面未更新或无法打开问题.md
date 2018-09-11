---
title: 部署hexo后github pages页面未更新或无法打开问题
date: 2018-03-30 15:34:29
categories:
- methods
tags:
- hexo
- github pages
---



# 部署

本地编写完成后，要同步Github Pages，运行以下三个命令即可：

```
hexo clean
hexo g
hexo d
```

需要注意的是**这些命令应该在hexo目录的根目录下进行**。

# 部署后页面未更新

使用**无痕模式**浏览或者**等一段时间**再查看。



---

更推荐运行`hexo s`后利用本地服务器在**localhost:4000**查看效果，效果满意后直接部署，过段时间页面自然会更新的。