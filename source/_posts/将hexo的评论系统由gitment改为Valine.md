---
title: 将hexo的评论系统由gitment改为Valine
toc: false
date: 2018-09-13 15:10:56
categories:
- methods
tags:
- hexo
- gitment
- Valine
---

首先注册[LeanCloud](https://leancloud.cn/)，注册后添加应用，然后选择`应用>设置>应用key`就可以看到自己的AppID和AppKey了。

然后进入自己的主题目录（比如我的主题是默认的`landscape`）：

删除配置gitment时`/themes/landscape/layout/_partial/post`目录下添加的`git.ejs`文件，

然后编辑`/themes/landscape/layout/_partial/`目录下的`article.ejs`，将原本配置gitment时添加在最后的那段代码删掉，添加：

```ejs
<% if (!index){ %>
  <% if (post.comments){ %>
    <div id="vcomments"></div>
    <script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
    <script src='//unpkg.com/valine/dist/Valine.min.js'></script>
    <script>
        new Valine({
            el: '#vcomments',
            appId: '你的appid',
            appKey: '你的appkey',
            notify:true, 
            verify:true, 
            visitor:true,
            avatar:'mm', 
            placeholder: '嘻嘻嘻' 
        })
    </script>
  <% } else { %>
    <div class="vcomments"></div>
  <% } %>
<% } %>
```

其中notify为邮件提醒功能是否开启，verify为验证码功能，visitor为文章访问量统计功能，[avatar](https://valine.js.org/avatar.html)为`Gravatar` 头像展示方式。

然后即OK啦！！

有其他问题可以访问[Valine官方文档](https://valine.js.org/quickstart.html)查看。



关于出现`Code 403: 访问被api域名白名单拒绝，请检查你的安全域名设置.`的问题：

我的问题是同时在github和coding上部署了，但是在leancloud的`应用>设置>安全中心>Web安全域名`中只添加了github的域名，因此在coding的那个域名访问时就会出现上述问题，添加域名即可解决问题。