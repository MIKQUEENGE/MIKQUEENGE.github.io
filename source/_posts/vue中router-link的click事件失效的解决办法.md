---
title: vue中router-link的click事件失效的解决办法
toc: false
date: 2018-12-04 16:28:49
categories:
- Web
tags:
- vue
---

使用`@click.native`

<!-- more -->

**问题原因：**

`router-link`会阻止`click`事件

`.native`指直接监听一个原生事件