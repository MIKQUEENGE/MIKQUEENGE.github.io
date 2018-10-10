---
title: 关于hexo博客自定义域名后gitment评论系统登陆出现redirect error返回主页的解决办法
toc: false
date: 2018-04-16 22:57:50
categories:
- methods
tags:
- hexo
- gitment
- OAuth
- 阿里云
---

>  背景：
>
> 原地址：`https://mikqueenge.github.io`
>
> 新域名：`http://blog.zmj97.top`(这里一定要注意！从阿里云买的域名使用的协议是http！)

<!-- more -->

今天下午兴致勃勃地买了个域名绑定到这个博客上后，发现昨天好不容易跳了各种坑才实现的**评论功能无法登陆**了！

每一次点击评论里的登陆都会回到index页面，地址栏显示地地址为`https://blog.zmj97.top/?error=redirect_uri_mismatch&error_description=The+redirect_uri+MUST+match+the+registered+callback+URL+for+this+application.&error_uri=https%3A%2F%2Fdeveloper.github.com%2Fv3%2Foauth%2F%23redirect-uri-mismatch`

一看这个提示，`redirect error` 让人不禁想到OAuth应用注册时填写的`Authorization callback URL`回调URL，

各种百度谷歌了一下，如果自定义了域名的话<u>回调URL要填写自定义域名</u>，而且**一个字符都不能出错**！否则就会出现上述无法登陆的情况...

试了一晚上，回调URL可能出现的字符错误有以下几个：

- 多加空格
- 协议错误，区分http和https
- 多加/

最后终于成功时我的OAuth应用信息的究极形态如下：

![](/images/Capture5.PNG)

心好累...