---
title: hexo博客谷歌百度收录踩坑日记
toc: false
date: 2018-04-17 00:09:38
categories:
- methods
tags:
- hexo
- 网站收录
---

## 百度收录文件验证

无论怎么把渲染关掉或者render_skip都说我的格式错误，看了一下源代码发现即使不渲染最后也会加上html的标签，于是放弃这个放弃了这个方式。

<!-- more -->

## 百度收录html验证

本来以为这个应该会直接就验证通过了，但是只要我修改了html，百度就无法访问我的博客，遂也放弃了这个方法..

## 百度收录CNAME验证

使用阿里云进行云解析但是阿里云现在不支持xxx.github.io的域名...

于是踏上了新征程：

## 自定义域名

在阿里云买了一个最便宜的.top域名，把自定义域名和博客绑定上之后博客就无法访问了，需要细心等待，谷歌了一下一般不会超过48h就会绑定成功可以正常使用。

阿里云的速度挺快，不到一个小时就好了。

弄好自定义域名之后就悲催地发现评论板块无法登陆...磕磕绊绊[改好配置](http://blog.zmj97.top/2018/04/16/关于hexo博客自定义域名后gitment评论系统登陆出现redirect-error返回主页的解决办法)后，终于开始重新进行百度收录了！

## 谷歌收录

由于白天的阴影先弄了谷歌收录，没有遇到什么大坑，一切都非常顺利，直到上sitemap时出现了两个问题：

### sitmap.xml不存在

安装sitemap插件时一定要加上`--save`！！：

```shell
npm install hexo-generator-sitemap --save
```

而不是

```shell
npm install hexo-generator-sitemap
```

### 测试sitmap.xml出现错误：此位置的 Sitemap 不允许此网址

搜了一下，各家有各家的错误原因，我的是因为我在谷歌收录的网址是原网址`https://mikqueenge.github.io`，而上传的sitemap.xml的地址自动被解析为自定义域名`http://blog.zmj97.top/sitemap.xml`才出现了错误，再添加收录网站`http://blog.zmj97.top`然后在这个地址下添加sitemap即可。

## 百度收录

### `token`

数据引入->链接提交->自动提交->主动推送（实时）->推送接口 中的接口调用地址中有token的值。

### 自动抓取sitemap失败

直接访问提交的数据文件地址`http://blog.zmj97.top/baidu_sitemap.txt`是可以看到的，但是因为 GitHub 屏蔽了百度的爬虫所以百度无法抓取...

然后发现我的配置跟主动推送的配置（[参考链接](http://www.yuan-ji.me/Hexo-优化%EF%BC%9A提交sitemap及解决百度爬虫抓取-GitHub-Pages-问题/)）很像，但是deploy baidu_submitter一直出错，看了错误信息才发现是因为把`baidu_url_submit:`下的`path: baidu_urls.txt`擅自改了文件名导致的...

终于好了...踩坑结束！