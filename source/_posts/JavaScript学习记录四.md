---
title: JavaScript学习记录四
toc: true
date: 2018-09-16 20:31:22
categories:
- Web
tags:
- JavaScript
---

——《JavaScript高级程序设计（第2版）》学习笔记

要多查阅[MDN Web 文档](https://developer.mozilla.org/zh-CN/docs/Web)

---

# BOM

Browser Object Model，浏览器对象模型。

BOM提供了很多用于访问浏览器的功能，这些功能与任何网页内容无关。

BOM缺少事实上的规范，因此浏览器之间共有的对象就成了事实上的标准。

没有所谓的标准BOM实现或者标准BOM接口。

图片来源于网络：

![BOM结构图](http://www.splessons.com/wp-content/uploads/2016/03/javascript-bom-01-splessons-1.png)

## window对象

[window文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)

BOM的核心对象是window，表示浏览器的一个实例。

在浏览器中，window对象有双重角色：

- 通过JavaScript访问浏览器窗口的一个接口
- ECMAScript规定的Global对象

因此window对象有权访问parseInt()等方法。

### 全局作用域

因为window对象又是ECMAScript中的Global对象，因此在全局作用域中声明的所有变量、函数都会变成window对象的属性和方法。

在全局作用域中，this指向window。

### ！窗口关系及框架

**因为书中使用的frameset和frame已经被HTML5废弃，用iframe取代，因此在看了HTML5后再来补充这一部分。**

top、parent、一个框架一个window对象

### ！窗口位置

screenLeft、screenX、moveTo等属性等看完文档再来详细记。

### ！窗口大小

innerWidth、outerWidth、clientWidth、pageWidth、resizeTo()等属性等看完文档再来详细记。

### 导航和打开窗口

window.open() 方法用于打开一个新的浏览器窗口或查找一个已命名的窗口，返回指向新窗口的引用。

`window.open(URL,name,specs,replace)`，具体格式看[这里](http://www.runoob.com/jsref/met-win-open.html)。

新创建的window对象有一个opener属性，保存着打开它的原始窗口对象。

#### 安全限制

弹窗广告问题。

为了解决这个问题，有些浏览器开始在弹出窗口配置方面增加限制。

#### 弹出窗口屏蔽程序

弹出窗口被屏蔽有两种可能：

- 被浏览器内置的屏蔽程序阻止，则window.open()很有可能返回null
- 被浏览器扩展或其他程序阻止，则window.open()通常会抛出异常

因此要想准确地检测弹出窗口是否被屏蔽：

```js
var blocked = false;
try {
  var popup = window.open("https://blog.zmj97.top", "_blank");
  if (popup == null) {
    blocked = true;
  }
} catch(e) {
  blocked = true;
}
if (blocked) {
  alert("The Popup was blocked!");
}
```

检测弹出窗口是否被屏蔽并不会阻止浏览器显示与被屏蔽窗口的相关信息。

### 超时调用

setTimeout()函数接受两个参数，要执行的代码和执行代码前要等待多少毫秒。

第一个参数可以是一个包含JavaScript代码的字符串（不推荐，就和在eval()函数中使用的字符串一样），

也可以是一个函数：

```js
setTimeout("alert('Hello World!')", 1000); // 不推荐

// 推荐的调用方式
setTimeout(function() {
  alert('Hello World!');
}, 1000);
```

调用setTImeout()后，该方法会返回一个数值ID，表示超时调用，

可以用它作为参数调用clearTimeout()来取消超时调用。

超时调用ID是计划执行代码的唯一标识符。

```js
// 设置超时调用
var timeoutId = setTimeout(function() {
  alert('Hello World!');
}, 1000);
// 把它取消
clearTimeout(timeoutId);
```

超时调用的代码都是在全局作用域中执行的，因此函数中的this的值通常指向window对象。

### 间歇调用

setInterval()函数接受两个参数，要执行的代码和每次执行代码前要等待多少毫秒。

```js
setInterval("alert('Hello World!')", 1000); // 不推荐

// 推荐的调用方式
setInterval(function() {
  alert('Hello World!');
}, 1000);
```

取消间歇调用：

```js
var num = 0;
var max = 10;
var intervalId = null;

function incrementNumber() {
  num++;
  // 如果执行次数到达max，则取消间歇调用
  if (num == max) {
    clearInterval(intervalId);
    alert("Done");
  }
}

intervalId = setInterval(incrementNumber, 500);
```

实际上，使用超时调用来模拟间歇调用被认为是最佳模式，因为后一个间歇调用可能会在前一个间歇调用结束之前启动。

```js
var num = 0;
var max = 10;

function incrementNumber() {
  num++;
  // 如果执行次数未到达max，则设置另一次超时调用
  if (num < max) {
    setTimeout(incrementNumber, 500);
  } else {
    alert("Done");
  }
}

setTimeout(incrementNumber, 500);
```

### 系统对话框

`alert()`只有一个确认按钮

`confirm()`有确认和取消两个按钮，返回true表示点了确认，false表示点了取消

`prompt()`，提示框，有一个文本输入域和确认取消按钮，两个参数为要显示给用户的提示内容和文本输入域内的默认内容，如果点击确认则返回文本输入域的值，否则返回null。

三者均不涉及HTML、CSS、JavaScript。

还有含有复选框的对话框选择是否阻止后续的对话框的显示。

还可以在JavaScript中通过`window.print()`和`window.find()`来显示打印和查找对话框。这两者是异步显示的，因此对话框计数器的不会吧它们计算在内。

## location对象

提供了与当前窗口中加载的文档有关的信息和一些导航功能。

location对象既是window对象的属性，有事document对象的属性。

location将URL解析为独立的片段。

下面是location对象的所有属性（忽略了每个属性前的location前缀）：

| 属性名   | 例子                     | 说明                                                         |
| -------- | ------------------------ | ------------------------------------------------------------ |
| hash     | "#contents"              | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串 |
| host     | "blog.zmj97.top:80"      | 返回服务器名称和端口号（如果有）                             |
| hostname | "blog.zmj97.top"         | 返回不带端口号的服务器名称                                   |
| href     | "https://blog.zmj97.top" | 返回当前加载页面的完整URL。location对象的toString()方法也返回这个值 |
| pathname | "/tag/"                  | 返回URL中的目录和/或文件名                                   |
| port     | "8080"                   | 返回URL指定的端口号。如果URL中不包含端口号，则返回空字符串   |
| protocol | "https:"                 | 返回页面使用的协议，通常是http:或https:                      |
| search   | "?q=javascript"          | 返回URL的查询字符串。这个字符串以问号开头                    |

### 查询字符串参数

尽管location.search返回从问号到URL末尾的所有内容，但却没有办法逐个访问每个查询字符串参数，因此可以：

```js
function getQueryStringArgs() {
  // 取得查询字符串并去掉开头的问号
  var qs = (location.search.length > 0 ? location.search.substring(1) : "");
  // 保存数据的对象
  var args = {};
  // 取得每一项
  var items = qs.split("&");
  var item = null,
      name = null,
      value = null;
  // 逐个添加到args中
  for (var i = 0; i < items.length; i++) {
    item = items[i].split("=");
    name = decodeURIComponent(item[0]);
    value = decodeURIComponent(item[1]);
    args[name] = value;
  }

  return args;
}
```

### 位置操作

`location.assign("https://blog.zmj97.top")`，立即打开新URL并在浏览器的历史记录中生成一条记录。

`window.location = "https://blog.zmj97.top"`和`location.href = "https://blog.zmj97.top"`与调用assign()的效果一样。

每次修改location对象的属性，页面都会以新URL重新加载（hash除外），并在浏览器的历史记录中生成一条新纪录（包括hash）。

使用`location.replace("https://blog.zmj97.top")`加载新页面后不会生成历史记录，也不能后退。

`location.reload()`重新加载，有可能从缓存中加载

`location.reload(true)`从服务器重新加载

位于reload()调用之后的代码可能会也可能不会执行，取决于网络延迟或系统资源等因素。

因此最好将reload()放在代码的最后一行。

## navigator对象

用于识别客户端浏览器，包含有关浏览器的信息。

### 检测插件

navigator.plugins数组的每一项包含下列属性：

- name：插件名字
- description：插件描述
- filename：插件文件名
- length：插件所处理的MIME类型数量

```js
// 检查插件（IE中无效）
function hasPlugin(name) {
  name = name.toLowerCase();
  for (var i = 0; i < navigator.plugins.length; i++) {
    if (navigator.plugins[i].toLowerCase().indexOf(name) > -1) {
      return true;
    }
  }

  return false;
}
```

每个插件对象本身也是一个MimeType对象的数组，包括四个属性：

- MIME类型描述description
- 回指插件对象的enablePlugin
- MIME类型对应的文件扩展名的字符串suffixes（以逗号分割）
- 完整MIME类型字符串type

在IE中检查插件只能使用专有的ActiveXObject类型，还要知道插件的COM标识符。

plugins集合有一个refresh()方法用于刷新插件，传入true会加载包含插件的所有页面。否则只更新插件不重新加载页面。

### 注册处理程序

registerContentHandler()、registerProtocalHandler()

为站点指明处理特定类型的信息

## screen对象

所有浏览器都支持的属性：

- availHeight：可用的屏幕高度（像素高度-系统部件高度），只读
- availWidth：可用的屏幕宽度（像素宽度-系统部件宽度），只读
- colorDepth：用于表现颜色的位数，多数系统都是32位，只读
- height：屏幕的像素高度
- width：屏幕的像素宽度

## history对象

保存用户上网的历史记录

使用go()方法可以在用户的历史记录中任意跳转

```js
history.go(-1); // 后退一页
history.go(2); // 前进两页
history.back(); // 后退一页
history.forward(); // 前进一页
```

也可以传入一个字符串：浏览器会跳转到历史记录中最近包含该字符串的页面

```js
history.go("wrox.com"); // 跳转到最近的wrox.com页面
```

