---
title: 接入gitment为hexo添加评论功能
toc: false
date: 2018-04-16 10:59:56
categories:
- methods
tags:
- hexo
- gitment
---

## 注册一个OAuth application 

[注册链接](https://github.com/settings/applications/new)

![](/images/Capture3.PNG)

其中：

Application name 为应用名，取一个跟自己博客相关的名字即可；

Homepage URL 为博客地址，例如我的为：https://mikqueenge.github.io；

Application description 为应用描述，可不填；

Authorization callback URL 为回调URL，可不填；



点击 Register application 祝成功后会得到这个应用的 **client ID** 和 **client secret**，等下配置文件时会用到。



## 配置文件 

主题：landscape

### 创建git.ejs

在`themes/landscape/layout/_partial/post`文件夹中创建文件`git.ejs`，写入下面的代码：

```ejs
<!-- Gitment评论插件通用代码 -->
<div id="git"></div>
<!-- 汉化版 -->
<link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
<script src="https://billts.site/js/gitment.js"></script>
<script>
var gitment = new Gitment({
  id: '{{ page.date }}', //添加此句解决Error：validation failed的问题
  owner: "%%%%%%%",//github用户名，例如MIKQUEENGE
  repo: "%%%%%%%",//用户存储评论的github项目名称，例如MIKQUEENGE.github.io
  oauth: {
    client_id: "%%%%%%%%%%%%",//注册OAuth Application时生产的ClinetID
    client_secret:"%%%%%%%%%%",//注册OAuth Application时生成的Client Secret
  },
})
gitment.render('git')
</script>
<!-- Gitment代码结束 -->
```

### 配置article.ejs

在`themes/landscape/layout/_partial/article.ejs`文件的结尾添加：

```ejs
<% if (!index){ %>
  <% if (post.comments){ %>
  <%- partial('post/git') %>
  <% } else { %>
    <div class="git"></div>
  <% } %>
<% } %>
```

## 登陆与添加评论

完成上述配置后部署并打开某篇文章，拉到最底部可以看到评论区：

![](/images/Capture4.PNG)

点击登陆后就可以添加评论啦！

## 遇到问题Error：validation failed

md文件名太长导致id出现问题，使用上述代码是不会出现这个问题的。

如果出现这个问题，解决方案为在gitment配置文件（如上述的`git.ejs`）中的`var gitment = new Gitment({})`内添加`id: '{{ page.date }}',`（不要忘记这个逗号）



---

参考链接：

[Hexo博客yelee主题添加Gitment评论系统 ](https://blog.csdn.net/stven_king/article/details/78357753)

[Gitment评论功能接入踩坑教程](http://ihtc.cc/2018/02/25/2018-02-25%20_Gitment评论功能接入踩坑教程/)