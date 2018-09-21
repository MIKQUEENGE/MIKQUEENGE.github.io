---
title: Web前端错题模糊题记录
toc: true
date: 2018-09-20 10:04:36
categories:
- Web
tags:
- HTML
- CSS
- JavaScript
---

## HTML

<u>**元素的alt和title有什么异同？**</u>

alt和title同时设置的时候，alt作为图片的替代文字出现，title是图片的解释文字。

**<u>关于html5标签？</u>**

\<audio\> 标签定义声音，比如音乐或其他音频流。
\<canvas\> 标签定义图形，比如图表和其他图像。\<canvas\> 标签只是图形容器，必须使用脚本来绘制图形。
\<article\>标签定义外部的内容。比如来自一个外部的新闻提供者的一篇新的文章，或者来自 blog 的文本，或者是来自论坛的文本。亦或是来自其他外部源内容。
\<menu\> 标签定义命令的列表或菜单。\<menu\> 标签用于上下文菜单、工具栏以及用于列出表单控件和命令。command 元素表示用户能够调用的命令。\<command\> 标签可以定义命令按钮，比如单选按钮、复选框或按钮。只有当 command 元素位于 menu 元素内时，该元素才是可见的。否则不会显示这个元素，但是可以用它规定键盘快捷键。 

**<u>有关HTML的Doctype和严格模式与混杂模式？</u>**

> **文档类型**
>
>  DTD（文档类型定义）是一组机器可读的规则，他们定义 XML 或 HTML 的特定版本中允许有什么，不允许有什么。在解析网页时，浏览器将使用这些规则检查页面的有效性并且采取相应的措施。浏览器通过分析页面的 DOCTYPE 声明来了解要使用哪个 DTD ，由此知道要使用 HTML 的哪个版本。
>
>  DOCTYPE 当前有两种风格，严格（ strict ）和过渡（ transitional ）。过渡 DOCTYPE 的目的是帮助开发人员从老版本迁移到新版本。
>
> 如果发送具有正确的 MIME 类型的 XHTML 文档，理解 XML 的浏览器将不显示无效的页面。
>
>  **浏览器模式**
>
>     浏览器有两种呈现模式：标准模式和混杂模式（quirks mode，也叫兼容模式）。在标准模式中，浏览器根据规范呈现页面；在混杂模式中，页面以一种比较宽松的向后兼容的方式显示。
>
> ** DOCTYPE 切换 **
>
> 对于 HTML 4.01 文档，
>
> - 包含严格 DTD 的 DOCTYPE 常常导致页面以标准模式呈现。
> - 包含过度 DTD 和 URI 的 DOCTYPE 也导致页面以标准模式呈现。
> - 但是有过度 DTD 而没有 URI 会导致页面以混杂模式呈现。
> - DOCTYPE 不存在或形式不正确会导致 HTML 和 XHTML 文档以混杂模式呈现。
> 

## CSS

**<u>关于CSS的position属性？</u>**

static没有定位，元素出现在正常的流中。

fixed是相对于窗口的固定定位。

**<u>关于border:none以及border:0?</u>**

当定义border:none时，表示无边框样式，浏览器并不会对边框进行渲染，也就没有实际的宽度；

定义边框时，除了设置宽度外，还必须设置边框的样式才能显示出来。

**<u>关于CSS sprites图片字节？</u>**

CSS Sprites能减少图片的字节，曾经比较过多次，3张图片合并成1张图片的字节总是小于这3张图片的字节总和。

**<u>关于浏览器引擎？</u>**

Wekbit是一个开源的Web浏览器引擎，也就是浏览器的内核。Apple的Safari, Google的Chrome, Nokia S60平台的默认浏览器，Apple手机的默认浏览器，Android手机的默认浏览器均采用的Webkit作为器浏览器内核。Webkit的采用程度由 此可见一斑，理所当然的成为了当今主流的三大浏览器内核之一。

另外两个分别是Gecko和Trident，大名鼎鼎的Firefox便是使用的Gecko 内核，而微软的IE系列则使用的是Trident内核。

还有Presto: Opera的内核，但由于市场选择问题，主要应用在手机平台--Opera mini。

另外，搜狗浏览器是双核的，双核并不是指一个页面由2个内核同时处理,而是所有网页（通常是标准通用标记语言的应用超文本标记语言）由webkit内核处理,只有银行网站用IE内核。

## JavaScript

<u>**flash和js通过什么类如何交互?**</u>

ExternalInterface。Flash提供了ExternalInterface接口与JavaScript通信，ExternalInterface有两个方法，call和addCallback，call的作用是让Flash调用js里的方法，addCallback是用来注册flash函数让js调用。

**<u>有关浏览器中使用js跨域获取数据？</u>**

只要<u>协议 、 域名 、 端口</u>有任何一个不同, 都被当作是 **不同** 的域。

> **1.CORS**
>
> CORS（Cross-Origin Resource Sharing，跨资源共享），基本思想是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应的成功或失败。即给请求附加一个额外的Origin头部，其中包含请求页面的源信息（协议、域名和端口），以便服务器根据这个头部决定是否给予响应。
>
> **2.document.domain**
>
> 将页面的document.domain设置为相同的值，页面间可以互相访问对方的JavaScript对象。
>
> 注意：
>
> 不能将值设置为URL中不包含的域；
>
> 松散的域名不能再设置为紧绷的域名。
>
> **3.图像Ping**
>
> var img=new Image();
>
> img.onload=img.onerror=function(){
>
> ... ...
>
> }
>
> img.src="url?name=value";
>
> 请求数据通过查询字符串的形式发送，响应可以是任意内容，通常是像素图或204响应。
>
> 图像Ping最常用于跟踪用户点击页面或动态广告曝光次数。
>
> 缺点：
>
> 只能发送GET请求；
>
> 无法访问服务器的响应文本，只能用于浏览器与服务器间的单向通信。
>
> **4.Jsonp**
>
> var script=document.createElement("script");
>
> script.src="url?callback=handleResponse";
>
> document.body.insertBefore(script,document.body.firstChild);
>
> JSONP由两部分组成：回调函数和数据
>
> 回调函数是接收到响应时应该在页面中调用的函数，其名字一般在请求中指定。
>
> 数据是传入回调函数中的JSON数据。
>
> 优点：
>
> 能够直接访问响应文本，可用于浏览器与服务器间的双向通信。
>
> 缺点：
>
> JSONP从其他域中加载代码执行，其他域可能不安全；
>
> 难以确定JSONP请求是否失败。
>
> **5.Comet**
>
> Comet可实现服务器向浏览器推送数据。
>
> Comet是实现方式：长轮询和流
>
> 短轮询即浏览器定时向服务器发送请求，看有没有数据更新。
>
> 长轮询即浏览器向服务器发送一个请求，然后服务器一直保持连接打开，直到有数据可发送。发送完数据后，浏览器关闭连接，随即又向服务器发起一个新请求。其优点是所有浏览器都支持，使用XHR对象和setTimeout()即可实现。
>
> 流即浏览器向服务器发送一个请求，而服务器保持连接打开，然后周期性地向浏览器发送数据，页面的整个生命周期内只使用一个HTTP连接。
>
> **6.WebSocket**
>
> WebSocket可在一个单独的持久连接上提供全双工、双向通信。
>
> WebSocket使用自定义协议，未加密的连接时ws://；加密的链接是wss://。
>
> var webSocket=new WebSocket("ws://");
>
> webSocket.send(message);
>
> webSocket.onmessage=function(event){
>
> var data=event.data;
>
> ... ....
>
> }
>
> 注意：
>
> 必须给WebSocket构造函数传入绝对URL；
>
> WebSocket可以打开任何站点的连接，是否会与某个域中的页面通信，完全取决于服务器；
>
> WebSocket只能发送纯文本数据，对于复杂的数据结构，在发送之前必须进行序列化JSON.stringify(message))。
>
> 优点：
>
> 在客户端和服务器之间发送非常少的数据，减少字节开销。

**<u>如何获取一个元素节点（id 为 test）的父元素，找到之后如何删除这个元素节点（id 为 test）？</u>**

```js
var testNode = document.getElementById('test');
var parentNode = testNode.parentNode;
parentNode.removeChild(testNode);	
```

**<u>!编写一个 js 函数 jsonp 的处理函数:</u>**

[讲解链接](https://blog.csdn.net/u013830811/article/details/52718664)

```js
// 手写jsonp
function myCallback(data) {
    console.log(data)
}
 
function jsonp(url, data, callback) {
    // data是否是字符串，是的话证明data值就是函数名
    if (typeof data == 'string') {
        callback = data
        data = {}
    }
    // 拼接data
    url += url.indexOf('?') === -1 ? '?' : '&'
    url += 'callback=' + callback
    var params = ""
    for (var i in data) {
        params += '&' + i + '=' + data[i]
    }
    url += params
    // 在页面插入script标签
    var script = document.createElement('script')
    script.setAttribute('src', url)
    document.querySelector('head').appendChild(script)
 
}
 
jsonp('http://baidu.com/index.html', { id: 34 }, 'myCallback')
jsonp('http://baidu.com/index.html?name="zjn"', { id: 34 }, 'myCallback')
```

**<u>编写一个函数判断参数是否是数组类型，如果是返回 true</u>**

```js
// 方法一：
function isArray(arg){
  return (arg instanceof Array);
}
// 方法二：
function isArray(arg){
  return Object.prototype.toString.call(arg) == '[object Array]' ? true : false;
}
// 方法三：
function isArray(arg){
  return arg.__proto__.constructor.name == 'Array' ？true : false;
}
```

**<u>关于对象的length属性：</u>**

Window.length //返回在当前窗口中frames的数量（包括IFRAMES）

String.length //返回字符串中的字符数目

Function.length //获取一个函数定义的参数数目

Array.length //返回数组中元素的数目

**如何获取 url 中的 query 字段对应的值，比如：[https://m.mobike.com?source=part1](https://m.mobike.com/?source=part1)，编写一个函数获取 source 对应的值 part1**

```js
let query = (url) => {
  let p = url.split('?')[1];
  let sourcePos = p.indexOf('source');
  if (sourcePos > -1) {
    if (p.indexOf('&', sourcePos+7) > -1) {
      return p.substring(sourcePos+7,p.indexOf('&', sourcePos+7));
    } else {
      return p.substring(sourcePos+7);
    }
  }
};
```

**<u>关于IE的event对象支持的方法：</u>**

IE的所有事件对象都支持的方法和属性：

- cancelBubble 默认为false，设置为true就可以取消事件冒泡
- returnValue 默认为true，设置为false可以取消事件的默认行为
- srcElement 对于生成事件的 Window 对象、Document 对象或 Element 对象的引用
- type 被触发事件的类型

DOM事件的方法（IE的事件模型不支持）：

- initEvent() 初始化新创建的 Event 对象的属性
- preventDefault() 通知浏览器不要执行与事件关联的默认动作
- stopPropagation() 不再派发事件

## 其他

**<u>和后端 API 服务通信的方式有哪些</u>**

1. ajax
2. websocket
3. SSE
4. 服务器端渲染

**<u>POST 提交的时候，content-type 有哪几种？</u>**

常见的有四种

1. application/x-www-form-urlencoded
2. application/json 
3. multipart/form-data
4. text/xml 

**<u>对前端工程化的理解，以及任意构建工具(webpack、gulp、grunt、rollup)的某一个使用的一些描述</u>**

**前端工程化**

https://blog.csdn.net/mayfla/article/details/78697020

**webpack**

webpack是一个前端模块化方案，更侧重模块打包，我们可以把开发中的所有资源（图片、js文件、css文件等）都看成模块，通过loader（加载器）和plugins（插件）对资源进行处理，打包成符合生产环境部署的前端资源。

**<u>Node.js 的核心模块</u>**

HTTP模块、URL模块、Query Strings模块、File System模块、Path模块、Global模块。