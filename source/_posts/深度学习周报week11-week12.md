---
title: 深度学习周报week11-week12
toc: true
date: 2018-06-27 20:22:11
categories:
- deep learning
tags:
---

## 实现本地服务器

## Tornado

利用tornado框架搭建本地服务器。

主要学习以下内容：

- define
- Future
- RequestHandler
- 协程装饰器
- http相关
- json相关
- 正则表达式匹配url


## 优化

继续调整参数和网络进行优化

## 整合

将训练模型单独整合为一个函数，server单独为一个文件，运行server，在开启本地服务器时，调用此函数训练模型，选定一个测试数据后直接进行预测。