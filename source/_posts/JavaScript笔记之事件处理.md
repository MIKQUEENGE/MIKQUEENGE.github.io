---
title: JavaScript笔记之事件处理
toc: true
date: 2018-10-17 20:55:28
categories:
- Web
tags:
- JavaScript
- 事件处理
---

——《JavaScript权威指南》**第十七章 事件处理** 读书笔记。

简单来说，

**事件（event）**就是Web浏览器通知应用程序发生了什么。

**事件类型（event type）**则是用来说明发生了什么类型事件的 **字符串**。

**事件目标**

**事件处理程序/事件监听程序**

**事件对象**

**事件传播**

**事件捕获**

<!-- more -->





## 注册事件处理程序

- 给目标元素设置属性
- 将事件处理程序传递给对象或元素的一个方法
  - IE8及以前版本——`attachEvent()`
  - 其他——`addEventListener()`

### 设置JavaScript对象属性为事件处理程序

事件处理程序属性的名字由`on`+事件名组成（且均为小写）：

`onclick`、`onchange`、`onload`、`onmouseover`等等。

```js
window.onload = function() {
  //...
}
```

**缺点**：每个目标对于同一个事件类型最多只能有一个处理程序。

### 设置HTML标签属性为事件处理程序

属性值为事件处理函数的主体。

```html
<button onclick="alert('Thank you');">
  点击这里
</button>
```

