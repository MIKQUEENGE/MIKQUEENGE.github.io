---
title: AJAX学习笔记
toc: true
date: 2018-10-11 12:55:52
categories:
- Web
tags:
- AJAX
---

AJAX，Asynchronous JavaScript and XML，异步JavaScript和XML。AJAX不是一个编程语言，而是一个用于快速创建动态网页的技术。AJAX使得在不重新加载整个页面的情况下与服务器交换数据并更新部分网页。

<!-- more -->

## AJAX的优缺点

- 优点
  - 不需要插件支持
  - 用户体验极佳
  - 提升Web程序性能
  - 减轻服务器和宽带的负担
- 缺点
  - 前进后退按钮被破坏
  - 搜索引擎的支持不够
  - 开发调试工具缺乏

## 工作原理

AJAX使用**XMLHTTPRequest**对象**异步**的与服务器交换数据。

AJAX与浏览器和平台无关。

XHR，XMLHttpRequest，可扩展超文本传输请求。

XMLHttpRequest 在后台与服务器交换数据，因此可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

![AJAX工作原理](https://7n.w3cschool.cn/attachments/image/20160225/1456389960559062.gif)

## 创建XHR对象

```js
var xmlhttp; 
if (window.XMLHttpRequest) {
  // code for IE7+, Firefox, Chrome, Opera, Safari 
  xmlhttp = new XMLHttpRequest(); 
} else {
  // code for IE6, IE5 
  xmlhttp = new ActiveXObject("Microsoft.XMLHTTP"); 
}
```

## 发送请求

| 方法                         | 描述                                           |
| ---------------------------- | ---------------------------------------------- |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。     |
| send(*string*)               | 将请求发送到服务器。*string*：仅用于 POST 请求 |

- ***method***：请求的类型；GET 或 POST
- ***url***：文件在服务器上的位置
- ***async***：true（异步）或 false（同步）

**GET 还是 POST？**

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

**GET**:

```js
xmlhttp.open("GET","demo_get.HTML",true);
xmlhttp.send();
/*
 *在上述代码中，可能得到的是缓存的结果
 *为了避免这种情况，可以向 URL 添加一个唯一的 ID：
 */
xmlhttp.open("GET","demo_get.html?t=" + Math.random(),true); 
xmlhttp.send();
// 如果希望通过GET方法发送信息，可以向URL添加信息
xmlhttp.open("GET","demo_get2.html？fname=Henry&lname=Ford",true); 
xmlhttp.send();
```

- GET 请求可被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可被收藏为书签
- GET 请求不应在处理敏感数据时使用
- GET 请求有长度限制（取决于浏览器的URL长度限制）
- GET 请求只应当用于取回数据

**POST**：

```js
xmlhttp.open("POST","demo_post.html",true); 
xmlhttp.send();
/*
 *如果需要像 HTML 表单那样 POST 数据，
 *可以使用 setRequestHeader() 来添加 HTTP 头
 *然后在 send() 方法中规定要发送的数据
 */
xmlhttp.open("POST","ajax_test.html",true); 
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
xmlhttp.send("fname=Henry&lname=Ford");
```

- POST 请求不会被缓存
- POST 请求不会保留在浏览器历史记录中
- POST 请求不能被收藏为书签
- POST 请求对数据长度没有要求

## readyState

XMLHttpRequest对象提供了 onreadystatechange 事件机制来捕获请求的状态。

`readyState`存有 XMLHttpRequest 的状态，从 0 到 4 发生变化：

- 0：请求未初始化，还没有调用 [open()](https://www.w3cschool.cn/ajax/ajax-xmlhttprequest-send.html)。 
- 1：请求已经建立，但是还没有发送，还没有调用 [send()](https://www.w3cschool.cn/ajax/ajax-xmlhttprequest-send.html)。  
- 2：请求已发送，正在处理中（通常现在可以从响应中获取内容头）。  
- 3：请求在处理中；通常响应中已有部分数据可用了，没有全部完成。  
- 4：响应已完成；您可以获取并使用服务器的响应了。 

每当 readyState 改变时，就会触发 onreadystatechange 事件。

即 onreadystatechange 事件会被触发 5 次（0 - 4），对应着 readyState 的每个变化。

`status`：

- 200: "OK" 
- 404: 未找到页面



## 响应

使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性来获取来自服务器的响应。

| 属性         | 描述                       |
| ------------ | -------------------------- |
| responseText | 获得字符串形式的响应数据。 |
| responseXML  | 获得 XML 形式的响应数据。  |

只有readyState值变为4时，responseText属性才可用。

```js
document.getElementById("myDiv").innerHTML = xmlhttp.responseText;

xmlDoc = xmlhttp.responseXML; 
txt=""; 
x = xmlDoc.getElementsByTagName("ARTIST"); 
for (i=0;i<x.length;i++) { 
  txt = txt + x[i].childNodes[0].nodeValue + "<br>"; 
} 
document.getElementById("myDiv").innerHTML=txt;
```

