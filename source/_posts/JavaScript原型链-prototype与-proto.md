---
title: 'JavaScript原型链:prototype与__proto__'
toc: false
date: 2018-09-04 11:16:54
categories:
- Web
tags:
- JavaScript
- prototype
---



主要看了[这一篇](https://www.w3cschool.cn/javascript/javascript-5isn2lax.html)，讲解的很清晰，最主要的一点为：



若：

```js
var Person = function () { };
var p = new Person();
```

则：

```js
p.__proto__ = Person.prototype;
```

当调用`p.xxx()`时，首先在`p`中找`xxx`这个属性，没有的话从`p`的`__proto__`(即`Person`的`prototype`)中寻找，如果没有，则继续向上寻找（`p.__proto__.__proto__`即`Person.prototype.__proto__`, ...）。





例如下面这个例子：

```js
var Person = function() {
    Person.prototype.Say = function () {
        alert("Person say");
    }
}

var Programmer = function() {}
Programmer.prototype = new Person();
var p = new Programmer();
```

我们可以得出：

`p.__proto__ = Programmer.prototype`

`p.__proto__.__proto__ = Programmer.prototype.__proto__ = Person.prototype`

所以在调用`p.Say()`方法时，现在`p`中寻找这个属性，如果没有就在`p.__proto__`中寻找，然后一步一步往上，最后在`Person.prototype`找到这个方法。



