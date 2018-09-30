---
title: cookie、sessionStorage和localStorage
toc: false
date: 2018-09-25 16:49:57
categories:
- Web
tags:
- cookie
- webstorage
---

## cookie

由于HTTP协议是无状态的，它自身不对请求和响应之间的通信状态进行保存，因此为了实现保持登录状态等功能，引入了**Cookie**。

Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态。

若不为Cookie设置过期时间，那么Cookie会在浏览器关闭时被删除。

因为Cookie被携带在http报文中，所以Cookie只适合存储比较小的数据，不能超过4KB。

## webstorage

HTML5提供的在客户端存储数据的方式。

webstorage有两种存储数据的方式：

- sessionStorage，针对一个session（会话）的存储
- localStorages，持久化的本地存储

容量上限：

| Feature        | Chrome | Firefox (Gecko) | Internet Explorer | Opera | Safari (WebKit) |
| -------------- | ------ | --------------- | ----------------- | ----- | --------------- |
| localStorage   | 4      | 3.5             | 8                 | 10.50 | 4               |
| sessionStorage | 5      | 2               | 8                 | 10.50 | 4               |

