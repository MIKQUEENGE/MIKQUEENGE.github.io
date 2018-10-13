---
title: JavaScript中Number常用属性和方法
toc: false
date: 2018-10-13 12:31:42
categories:
- Web
tags:
- JavaScript
---

`Number.MAX_VALUE`——1.7976931348623157e+308，可表示的最大数

`Number.MIN_VALUE`——5e-324，可表示的最小数

<!-- more -->

`toExponential(x)`——把对象的值转换为指数计数法

`toFixed(x)`——把数字转换为字符串，x为小数点后位数

`toPrecision(x)`——把数字格式化为指定的长度

`toString(x)`——使用x为基数，把数字转换为字符串

`valueOf()`——返回值

```js
var a = 5.4546;
a.toExponential(); // "5.4546e+0"
a.toExponential(2); // "5.45e+0"
a.toFixed(); // "5"
a.toFixed(5); // "5.45460"
a.toPrecision(3); // "5.45"
a.toString(); // "5.4546"
a.toString(2); //"101.01110100011000001010101001100100110000101111100001"
a.valueOf(); //5.4546
```

