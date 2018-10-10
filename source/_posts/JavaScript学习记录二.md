---
title: JavaScript学习记录二
toc: true
date: 2018-09-13 10:14:53
categories:
- Web
tags:
- JavaScript
---

——《JavaScript高级程序设计（第2版）》学习笔记

要多查阅[MDN Web 文档](https://developer.mozilla.org/zh-CN/docs/Web)

<!-- more -->

---

# 变量、作用域和内存问题

## 基本类型和引用类型的值

ECMAScript变量可能包含两种不同数据类型的值：

- **基本类型值**：保存在**栈内存**中的简单数据段，这种值完全保存在内存中的一个位置
- **引用类型值**：保存在**堆内存**中的对象，保存的实际上是一个指针，指针指向内存中真正对象保存的位置

五种基本数据类型：Undefined、Null、Boolean、Number、String在内存中占有固定大小的空间，因此可以保存在栈内存中。因为我们操作的是它们实际保存的值，所以它们是**按值**访问的。

对于对象，先从栈中读取内存地址，然后再按照地址找到保存在堆中的值。因为我们操作的不是实际的值，而是那个值所引用的对象，因此我们称之为**按引用**访问的。（图片来源于网络，cr 水印）

![栈内存与堆内存](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1536816598256&di=246ab4e73e12049d497d375df4fd273b&imgtype=jpg&src=http%3A%2F%2Fimg1.imgtn.bdimg.com%2Fit%2Fu%3D2929764421%2C2277861536%26fm%3D214%26gp%3D0.jpg)

### 动态属性

对于对象，我们可以改变和删除其属性和方法，但是不能给基本类型的值添加属性。

即只能给引用类型值动态地添加属性。

### 复制变量值

当复制基本类型值的时候，会在栈中为其开辟一块新的内存保存其值。

但是当复制引用类型的值时，实际上复制保存的是这个对象在堆内存中的地址，也就是两者指向的是同一个对象。

### 传递参数

ECMAScript中所有的函数的参数都是**按值传递**的。

传递基本类型值就如基本类型变量的复制一样，传递引用类型变量时也如同引用变量的复制。

因此传递引用类型的变量时，传递的相当于是拷贝的指针。

可以看下边这个例子（我觉得可以把对象看做是一个指向对象的指针，然后函数传递的是一个拷贝的指针）：

```javascript
function setName(obj) {
  obj.name = "Nicholas";
  obj = new Object();
  obj.name = "Greg";
}

var person = new Obejct();
setName(person);
alert(person.name); // "Nicholas"
```

### 检测类型

typeof检测null返回"object"

当我们想知道一个对象是什么类型的对象时，可以使用`instanceof`，它的语法是：

`result = variable instanceof constructor`

如果变量是给定引用类型（由构造函数表示）的实例，则instanceof返回true：

```javascript
alert(person instanceof Object); // 变量person是Object么？
alert(colors instanceof Array); // 变量colors是Array么？
```

当使用instanceof检测基本类型的值时始终返回false，因为基本类型不是对象。

注： 在Safiri和Chrome中使用typeof检测正则表达式会错误的返回"function"。

## 执行环境和作用域

[执行环境](https://blog.csdn.net/wmaoshu/article/details/60466990)定义了变量或函数有权访问的其他数据，决定了它们各自的行为。

每个执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。

在Web浏览器中，全局执行环境被认为是window对象，因此所有全局变量和函数都是作为window对象的属性和方法创建的。

一个执行环境中的所有代码执行完毕后，该环境和保存在其中的所有变量和函数定义都被销毁，

全局执行环境直到关闭页面或者浏览器时才会被销毁。

关于作用域链可以看[这里](https://blog.csdn.net/charlene0824/article/details/52252824)。

### 延长作用域链

- with语句： 其变量对象包含为指定的对象的所有属性和方法所做的变量声明。
- catch语句： 包含被抛出的错误对象的声明

在IE的JavaScript实现中，catch语句捕获的错误对象会被添加到执行环境的变量对象中。

### 没有块级作用域

- 使用if 、for语句创建的变量会保存在语句外部的执行环境中。

- 函数内部是一个局部环境
- 访问局部变量比全局变量快，因为不用向上搜索作用域链

## 垃圾收集

JavaScript具有自动垃圾收集机制，按照固定的时间间隔或代码执行中预定的收集时间，周期性地找出不再使用的变量释放其内存。

关于垃圾收集方式的详细解释可以看[这里](https://www.cnblogs.com/scottjeremy/p/6870729.html)。

关于性能问题和管理内存可以看[这里](https://www.cnblogs.com/yxField/p/4226591.html)。

# 引用类型

## Object类型

创建Object实例的方式：

- `var xxx = new Object()`
- `var xxx = {age: 29}`, `{}`是对象字面量边界

可以用`xxx.age`或者`xxx['age']`来访问属性，更建议用点表示法。

## Array类型

ECMAScript数组的每一项可以保存任何类型的数据。

数组的索引从0开始。

数组的项数保存在其length属性中，它并不是只读的，可以通过设置它来在数组的末尾移除或添加项，添加项的初始值为undefined。

因为JavaScript使用一个32位整数保存数组元素个数，因此数组最多可以包含4294967295项（2的32次方减1）。

以下是创建数组的例子：

```javascript
// new 可以省略
var a = new Array(); // 创建一个空数组
var b = new Array(20); // 创造一个包含20项的数组，每一项的初始值都是undefined
var c = new Array("red", "blue", "green"); // 创造一个包含三项："red","blue","green"的数组
var d = ["red", "blue", "green"]; // 创造一个包含三项："red","blue","green"的数组
var e = []; // 创建一个空数组
var f = [1,2,]; // 不要这样！这样会创建一个包含2或3项的数组
var g = [,,,,,]; // 不要这样！这样会创建一个包含5或6项的数组
```

### 转换方法

valueOf()返回当前对象的原始值；

toString()方法先调用每一项的toString()方法，然后用逗号将它们拼接起来并返回；

toLocalString()方法先调用每一项的toLocalString()方法，然后用逗号将它们拼接起来并返回；

如果使用join()方法可以使用不同的分隔符来构建这个字符串：

```javascript
var a = ['a','b','c'];
alert(a.join(',')); // 'a,b,c'
alert(a.join('||')); // 'a||b||c'
```

如果某一项是undefined或null，那么在toString()、toLocalString()、join()方法返回的结果中以空字符串表示。

### 栈方法

ECMAScript为数组提供了push()和pop()方法，以便实现类似栈的行为（LIFO，后进先出）。

- push()方法可以接受任意数量的参数，然后把它们逐个添加到数组末尾，并返回修改后的数组长度
- pop()方法则从数组末尾移除最后一项，并将length减一，返回移除的项。

### 队列方法

可以使用push()和shift()方法，实现类似于队列的行为（FIFO，先进先出）。

- shift()方法从数组开头移除第一项，并将length减一，返回移除的项。
- ECMAScript还提供了unshift()方法，可以接受任意数量的参数，然后把它们逐个添加到数组头部，并返回修改后的数组长度（IE返回undefined）。

关于unshift()的添加多个变量的顺序：

```javascript
a = ["a", "b", "c", null]；
a.unshift('1','2'); // ["1", "2", "a", "b", "c", null]
```

### 重排序方法

reverse()会反转数组项的顺序：

```javascript
var a = [1,2,6,5,3,0];
a.reverse();
alert(a); // 0,3,5,6,2,1
```

sort()为排序函数，默认从小到大排序，也可以传入一个比较函数：

```javascript
var a = [1,2,6,5,3,0];
a.sort();
alert(a); // 0,1,2,3,5,6

function compare(v1, v2) {
  if (v1 < v2) {
    return 1;
  } else if (v1 > v2) {
    return -1;
  } else {
    return 0;
  }
}
a.sort(compare);
alert(a); // 6,5,3,2,1,0
```

对于数值类型或者其valueOf()方法会返回数值类型的对象类型，可以简化一下比较函数：

```javascript
// 升序排序
function compare1(v1, v2) {
  return v1 - v2;
}

// 降序排序
function compare1(v1, v2) {
  return v2 - v1;
}
```

### 操作方法

#### concat()

——先创建一个当前数组的副本，然后将接收到的参数添加到这个副本的末尾，返回新构建的这个数组：

```javascript
var a = ['1','2','3'];
var b = a.concat('4',['5','6']);
alert(a); // 1,2,3
alert(b); // 1,2,3,4,5,6
```

#### slice()

——局部拷贝数组，接受一个参数或两个参数；

一个参数时拷贝从这个参数指定的位置到结尾；

两个参数时，拷贝两个参数之间的项，不包含结束为止的项。

当参数为负数时，则用该数加数组长度：

- 若加完之后大于等于零则用加完后的数计算
- 若加完之后还小于0则把该数看做0

若起始位置大于结束位置，则返回空数组。

下边为一个简单的例子：

```javascript
var a = [0,1,2,3,4];
var b = a.slice(1);
var c = a.slice(1,4);
alert(a); // 1,2,3,4
alert(b); // 1,2,3
```

#### splice()

主要用途是向数组的中部插入项，对原数组进行操作

使用方式主要有三种：

- 删除： 指定两个参数——要删除的第一项的位置和要删除的项数，返回被删除的项
- 插入： 指定三个参数——起始位置、0（要删除的项数）、要插入的项（任意数量），返回被删除的项（空）
- 替换： 指定三个参数——起始位置、要删除的项数、要插入的项（任意数量），返回被删除的项（空）

代码例子可以看[这里](https://blog.csdn.net/qq_33733970/article/details/78787726)。

## Date类型

是在早期Java中的java.util.Date类基础上构建的。

因此Date类型使用自UTC（国际协调时间）1970年1月1日零时开始经过的毫秒数来保存日期。

Date类型保存的日期能够精确到1970年1月1日前后285616年。

关于创建日期和设置日期可以看[这里](http://www.runoob.com/js/js-obj-date.html)。

Date对象属性及方法可以看[这里](http://www.runoob.com/jsref/jsref-obj-date.html)。

将表示日期的字符串传递给Date构造函数，后台会自动调用Date.parse()，然后将得到的值传给构造函数。

Date对象可以直接进行大小比较。

可以进行日期加减，下边的例子表示五天后的日期：

```javascript
var myDate=new Date();
myDate.setDate(myDate.getDate()+5);
```

## [RegExp类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

正则表达式： `var expression = / pattern / flags`

正则表达式语法看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp#Special_characters_in_regular_expressions)。

flags为一个或多个标志，正则表达式的匹配模式支持下面三个标志：

- g——全局(global)模式，应用于所有字符串
- i——不区分带小写(case-insensitive)模式
- m——多行(multiiline)模式，到达一行文本末尾时还会继续查找喜爱航是否存在与模式匹配的项

元字符：`()[]{}\^$|?*+.`，元字符必须转义。

以下是一些例子：

```javascript
var pattern1 = /at/g;		// 匹配字符串中所有"at"实例
var pattern2 = /.at/gi;		// 匹配字符串中所有以"at"结尾的三个字符的实例，不区分大小写
var pattern3 = /\.at/gi;	// 匹配字符串中所有".at"实例，不区分大小写
var pattern4 = /[bc]at/i;	// 匹配字符串中第一个"bat"或"cat"实例，不区分大小写
var pattern5 = /\[bc\]at/i;	// 匹配字符串中第一个"[bc]at"实例，不区分大小写
```

也可以使用构造函数来定义，例如下边两个式子得到的值是等价==相同的：

```javascript
var pattern1 = /[bc]at/i;
var pattern2 = new RegExp("[bc]at", "i");
```

需要注意的是，构造函数的字符串中，元字符必须双重转义，比如`/\./`双重转义为"\\\\."

### 实例属性

- global——布尔值，表示是否设置了g标志
- ignoreCase——布尔值，表示是否设置了i标志
- multiline——布尔值，表示是否设置了m标志
- lastIndex——整数，表示开始搜索下一个匹配项的字符位置，从0算起
- source——正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回

### RegExp实例方法

关于exec()方法看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)。

关于test()方法看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)。

### RegExp构造函数属性

| 长属性名     | 短属性名 | 说明                                     |
| ------------ | -------- | ---------------------------------------- |
| input        | $_       | 最近一次要匹配的字符串                   |
| lastMatch    | $&       | 最近一次的匹配项                         |
| lastParen    | $+       | 最近一次匹配的捕获组                     |
| leftContext  | $`       | input字符串中lastMatch之前的文本         |
| rightContext | $'       | input字符串中lastMatch之后的文本         |
| multiline    | $*       | 布尔值，表示是否所有表达式都使用多行模式 |

注：书中说Opera不支持input、lastMatch、lastParent、multiline，但查阅MDN文档显示的是支持的，因此待验证。

关于如何使用，以lastMatch为例：

```javascript
var re = /hi/g;
re.test('hi there!');
RegExp.lastMatch; // "hi"
RegExp['$&'];     // "hi"
```



另外对于书中提到的ECMAScript正则表达式不支持的特性，因为版本不断更新，比如现在已经支持Unicode，因此就不在这里列出来了。

## Function类型

所有函数实际上都是Function类型的实例，且与其他引用数据类型一样具有属性和方法。

**“函数是对象，函数名是指针”。**

定义函数的方式：

```javascript
function sum1(num1, num2) {
  return num1 + num2;
}

var sum2 = function(num1, num2) {
  return num1 + num2;
};

var sum3 = new Function('num1', 'num2', 'return num1 + num2'); // 不推荐
```

### 深入理解没有重载

将函数名想象为指针，当用同一个函数名重新声明一个函数实际上相当于改变了指针的指向。

指针只能指向一个对象。

### 函数声明与函数表达式

```javascript
// 函数声明
function sum1(num1, num2) {
  return num1 + num2;
}
// 函数表达式
var sum2 = function(num1, num2) {
  return num1 + num2;
};
```

解析器会率先读取函数声明，并使其在执行任何代码之前可用，

但是对于函数表达式，必须等到解析器执行到它所在的代码行才会真正被解释执行。

可以同时使用函数声明和函数表达式`var sum1 = function sum2() {}`，但会在Safari中导致错误。

### 作为值的函数

函数名本身就是变量，因此可以把函数当做参数传递，[这里](https://blog.csdn.net/lingfeng2008w/article/details/50598431)有人总结了当做参数传递的用法。

### 函数内部属性

在函数内部有两个特殊的对象：arguments和this。

**arguments**还有一个callee属性，该属性是一个指针，指向拥有这个arguments对象的函数，有什么用呢？可以看阶乘函数这个例子：

```javascript
function factorial(num) {
  if (num <= 1) {
    return 1;
  } else {
    return num * factorial(num-1);
  }
}
```

像这样递归，我们在修改函数名、拷贝函数后修改原函数内容后都会遇到麻烦，因此就要用到callee属性了：

```javascript
function factorial(num) {
  if (num <= 1) {
    return 1;
  } else {
    return num * arguments.callee(num-1); // 使用callee属性
  }
}

var trueFactorial = factorial;  // 拷贝函数
factorial = function() {  // 修改原函数定义
  return 0;
};

alert(factorial(5));  // 0
alert(trueFactorial(5));// 120
```

**this**是函数在执行时所处的作用域（挡在网页的全局作用域调用函数时，this对象引用的就是window），可以看下边这个例子：

```javascript
function sayColor() {
  alert(this.color);
}

window.color = 'red';
var b = {color: 'blue'};
b.sayColor = sayColor;

sayColor();   //'red'
b.sayColor(); //'blue'
```

### 函数属性和方法

每个函数都包含两个属性：length和prototype。

- length：函数希望接收的命名参数的个数
- prototype：可以看我[这篇文章](https://blog.zmj97.top/JavaScript%E5%8E%9F%E5%9E%8B%E9%93%BE-prototype%E4%B8%8E-proto.html)。

每个函数都包含两个非继承而来的方法：apply()和call()。

这两个的用途都是给函数**指定函数体内this的值**。

> `apply` 与 [`call()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 非常相似，不同之处在于提供参数的方式。
>
> `apply` 使用参数数组而不是一组参数列表。`apply` 可以使用数组字面量（array literal），如 `fun.apply(this, ['eat', 'bananas'])`，或数组对象， 如  `fun.apply(this, new Array('eat', 'bananas'))`。
>
> 而 `call`的语法为`fun.call(thisArg, arg1, arg2, ...)`。
>
> 需要注意的是，指定的`this`值并不一定是该函数执行时真正的`this`值，如果这个函数处于[非严格模式下](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Functions_and_function_scope/Strict_mode)，则指定为`null`和`undefined`的`this`值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的`this`会指向该原始值的自动[包装对象](https://www.cnblogs.com/moqing/p/5593986.html)。

每个函数都一个非标准的caller属性，指向调用该函数的函数，

因此一般在一个函数的内部，通过`arguments.callee.caller`来实现对调用栈的追溯，

但只建议将该属性用于调试目的。

## 基本包装类型

先看[这篇文章](https://www.cnblogs.com/moqing/p/5593986.html)

### Boolean类型

- 基本类型的布尔值： `var a = false`
- 引用类型的布尔值： `var b = new Boolean(false)`

除了包装对象的问题，两个还有两个区别：

- typeof 的结果一个是"boolean"一个是"object"
- instanceof测试是否为Boolean对象一个是false，一个是true

**建议永远不要使用Boolean对象。**

### Number类型

重写了valueOf()、toLocaleString()和toString()方法。

- valueOf()返回对象表示的基本类型的数值。

- 可以为toString()方法传递一个表示基数的参数，告诉它返回几进制数值的字符串形式。

- toFixed()方法会按照指定的小数位返回数值的字符串表示（四舍五入）：`var num = 10;num.toFixed(2)`结果为“10.00”
- toExponential()按照指定的小数位数返回数值的指数表示的字符串：`var num = 10;num.toExponential(2)`结果为“1.00e+1”
- toPrecision()接受一个参数作为表示数值所有数字的位数（不包括指数部分），然后返回最合适的表示格式的字符串。

### String类型

#### 字符方法

访问字符串中特定字符：`charAt()`、`charCodeAt()`

```javascript
var s = 'hello world!';
s.charAt(1);	// "e", 返回字符
s.charCodeAt(1);// "101"， 返回字符编码
s[1];			// "e"
```

#### 字符串操作方法

- concat() ： *string*.concat(*string1*, *string2*, ..., *stringX*)， 连接字符串，不改变原字符串，返回连接后的字符串
- slice()：*string*.slice(*start*,*end*)，提取字符串片断，start为要截取的片段的起始下标；end为要截取的片段的结尾下标加一，若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串，如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。
- substring()：类似于slice()
- substr()：*string*.substr(*start*,*length*)， 提取字符片段，start为要截取的片段的起始下标；length为要截取的长度，那么返回从 sstart到结尾的字串。

```javascript
var str1 = "Hello ";
var str2 = "world!";
str1.concat(str2); // "Hello world!"
str.slice(1,5); // "ello"
str.substring(1,5); // "ello"
str.substr(2,3); // "llo"
```

假设p为一个正确的坐标值，m为一个负值，则

- string.concat(p,m)x相当于string.concat(p)
- string.substring(p,m)相当于string.substring(p,0)相当于string.substring(0,p)
- string.substr(p,m)相当于string.substr(p,0)，返回空字符串

#### 字符串位置方法

`indexOf()`和`lastIndexOf()`，传入一个字符串，返回这个字符串在源字符串中第一次和最后一次出现的位置，若没有找到则返回-1。

还可以传入第二个参数表示开始查找的位置，`indexOf()`往后查找，`lastIndexOf()`往前查找。

#### 字符串大小写转换方法

- toLowerCase()、toUpperCase()
- toLocaleLowerCase()、toLocaleUpperCase()，针对地区应用不同的规则

#### 字符串的模式匹配方法

1. `match()`，等价于调用RegExp对象的exec()方法。

   `match()`接受一个正则表达式或者一个RegExp对象作为参数，返回一个数组，数组第一项是与整个模式匹配的字符串，之后的每一项都是和捕获组匹配的字符串。

2. `search()`与`match()`唯一不同的是返回的是第一个匹配项的索引。

3. `replace()`添加了一个传入的参数，表示匹配到的字符串要替换成的字符串。如果要全部替换，要记得在正则表达式中指定全局标志(g)。第二个参数也可以是函数，该函数的返回值将替换掉第一个参数匹配到的结果。

   替换字符串可以插入下面的特殊变量名：

| 变量名 | 代表的值                                                     |
| ------ | ------------------------------------------------------------ |
| `$$`   | 插入一个 "$"。                                               |
| `$&`   | 插入匹配的子串。                                             |
| $`     | 插入当前匹配的子串左边的内容。                               |
| `$'`   | 插入当前匹配的子串右边的内容。                               |
| `$*n*` | 假如第一个参数是 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp)对象，并且 n 是个小于100的非负整数，那么插入第 n 个括号匹配的字符串。 |

4. `splite()`，基于指定的分隔符`separator`将一个字符串分割成多个子字符串，还可以指定第二个参数，一个整数，限定返回的分割片段数量。

```javascript
"Webkit Moz O ms Khtml".split( " " )   // ["Webkit", "Moz", "O", "ms", "Khtml"]
var myString = "Hello World. How are you doing?";
myString.split(" ", 3); // ["Hello", "World.", "How"]
```

如果 `separator` 包含捕获括号（capturing parentheses），则其匹配结果将会包含在返回的数组中。

```js
var myString = "Hello 1 word. Sentence number 2.";
var splits = myString.split(/(\d)/);	// \d匹配数字

console.log(splits); // [ "Hello ", "1", " word. Sentence number ", "2", "." ]
```

#### localeCompare()方法

`referenceStr.localeCompare(compareString[, locales[, options]])`

判断字符串参数compareString是否在字母表中排在字符串referenceStr之前，是的话返回正数，不是返回负数，相等返回0。

locales和options都是可选参数，还没有被所有浏览器支持，具体的含义可以查阅[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)。

下边是一个简单的例子：

```js
// "c" 在 "a" 之后， 返回负数
'a'.localeCompare('c'); 
// -2 或者 -1 (或者其他负数)

// "against" 在 "check" 之前
'check'.localeCompare('against'); 
// 2 或者 1 (或者其他正数)

// 相同
'a'.localeCompare('a'); 
// 0
```

#### fromCharCode()方法

String构造函数的的静态方法，接收一个或者多个字符编码，然后把它们转换成一个字符串。

```js
String.fromCharCode(104, 101, 108, 108, 111); // "hello"
```

#### HTML方法

该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用这些特性。因此不再列出。

## 内置对象

### 全局(Global)对象

在大多是ECMAScript实现中都不能直接访问Global对象，不过Web浏览器实现了承担该角色的window对象，因此在全局作用域中生命的所有变量核函数，就都成为了window对象的属性。

#### 全局函数

所有在全局作用域定义的属性和方法都是Global对象的属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。

- [`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)：将传入的字符串当做 JavaScript 代码进行执行。
- [`uneval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/uneval) ：返回代表传入对象的源代码的字符串，该特性是非标准的，请尽量不要在生产环境中使用它！
- [`isFinite()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isFinite)：判定一个数字是否是有限数字。`isFinite` 方法检测它参数的数值。如果参数是 `NaN`，正无穷大或者负无穷大，会返回`false`，其他返回 `true`。
- [`isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)：确定一个值是否为[`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 。如果`isNaN`函数的参数不是`Number`类型， `isNaN`函数会首先尝试将这个参数转换为数值，然后才会对转换后的结果是否是[`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)进行判断。ECMAScript (ES2015)包含[`Number.isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)函数。通过`Number.isNaN(x)`来检测`变量x`是否是一个`NaN`将会是一种可靠的做法。然而，在缺少`Number.isNaN`函数的情况下, 通过表达式`(x != x)` 来检测`变量x`是否是`NaN`会更加可靠。
- [`parseFloat()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)：解析一个字符串参数并返回一个浮点数。
- [`parseInt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)：返回解析后的整数值。 如果被解析参数的第一个字符无法被转化成数值类型，则返回 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)。
- [`decodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)：解码一个由[`encodeURI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) 先前创建的统一资源标识符（URI）或类似的例程。
- [`decodeURIComponent()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) : 解码由 [`encodeURIComponent`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) 方法或者其它类似方法编码的部分统一资源标识符（URI）。
- [`encodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)：对统一资源标识符（URI）进行编码，将有效的URI不能包含的字符替换为特殊的UTF-8编码。
- [`encodeURIComponent()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) ：对统一资源标识符（URI）的组成部分进行编码的方法。它使用一到四个转义序列来表示字符串中的每个字符的UTF-8编码（只有由两个Unicode代理区字符组成的字符才用四个转义字符编码）。
- [`escape()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/escape) ，已废弃。生成新的由十六进制转义序列替换的字符串. 使用 [`encodeURI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) 或 [`encodeURIComponent`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) 代替。
- [`unescape()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/unescape) ，已废弃。

`encodeURI` 会替换所有的字符，但不包括以下字符，即使它们具有适当的UTF-8转义序列：

| 类型         | 包含                                          |
| ------------ | --------------------------------------------- |
| 保留字符     | `;` `,` `/` `?` `:` `@` `&` `=` `+` `$`       |
| 非转义的字符 | 字母 数字 `-` `_` `.` `!` `~` `*` `'` `(` `)` |
| 数字符号     | `#`                                           |

请注意，`encodeURI` 自身*无法*产生能适用于HTTP GET 或 POST 请求的URI，例如对于 XMLHTTPRequests, 因为 "&", "+", 和 "=" 不会被编码，然而在 GET 和 POST 请求中它们是特殊字符。

然而`encodeURIComponent` 转义除了字母、数字、`(`、`)`、`.`、`!`、`~`、`*`、`'`、`-`和`_`之外的所有字符。

例子：

```js
// 解码一个西里尔字母（Cyrillic）URL
decodeURI("https://developer.mozilla.org/ru/docs/JavaScript_%D1%88%D0%B5%D0%BB%D0%BB%D1%8B");
// "https://developer.mozilla.org/ru/docs/JavaScript_шеллы"

// 解码一个西里尔字母的URL
decodeURIComponent("JavaScript_%D1%88%D0%B5%D0%BB%D0%BB%D1%8B");
// "JavaScript_шеллы"

// *****************************************************************

var fileName = 'my file(2).txt';
var header = "Content-Disposition: attachment; filename*=UTF-8''" 
       + encodeRFC5987ValueChars(fileName);

console.log(header); 
// 输出 "Content-Disposition: attachment; filename*=UTF-8''my%20file%282%29.txt"

function encodeRFC5987ValueChars (str) {
  return encodeURIComponent(str).
    // 注意，仅管 RFC3986 保留 "!"，但 RFC5987 并没有
    // 所以我们并不需要过滤它
    replace(/['()]/g, escape). // i.e., %27 %28 %29
    replace(/\*/g, '%2A').
      // 下面的并不是 RFC5987 中 URI 编码必须的
      // 所以对于 |`^ 这3个字符我们可以稍稍提高一点可读性
      replace(/%(?:7C|60|5E)/g, unescape);
}
```

 注： [RFC 3986](http://tools.ietf.org/html/rfc3986)，保留 !, ', (, ), 和 *

#### Global对象的属性

特殊值如undefined等、所有原生引用类型的构造函数都是Global对象的属性。

除了这些还有：

- [`Error`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error)
- [`EvalError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/EvalError)
- [`InternalError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/InternalError)
- [`RangeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RangeError)
- [`ReferenceError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)
- [`SyntaxError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)
- [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)
- [`URIError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/URIError)

### Math对象

[**Math**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math) 是一个内置对象， 它具有数学常数和函数的属性和方法。不是一个函数对象。

`Math` 的所有属性和方法都是静态的：例如常数pi可以用 `Math.PI` 表示，用 `x` 作参数 Math.sin(x)调用sin函数。

#### Math对象的属性

[`Math.E`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/E)

欧拉常数，也是自然对数的底数, 约等于 2.718.

[`Math.LN2`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/LN2)

2的自然对数, 约等于0.693.

[`Math.LN10`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/LN10)

10的自然对数, 约等于 2.303.

[`Math.LOG2E`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/LOG2E)

以2为底E的对数, 约等于 1.443.

[`Math.LOG10E`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/LOG10E)

以10为底E的对数, 约等于 0.434.

[`Math.PI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/PI)

圆周率，一个圆的周长和直径之比，约等于 3.14159.

[`Math.SQRT1_2`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/SQRT1_2)

1/2的平方根, 约等于 0.707.

[`Math.SQRT2`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/SQRT2)

2的平方根,约等于 1.414.

#### 常用方法

[`Math.max()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

返回0个到多个数值中最大值.

[`Math.min()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/min)

返回0个到多个数值中最小值.

[`Math.ceil(x)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil)

返回x向上取整后的值.

[`Math.floor(x)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

返回小于x的最大整数。

[`Math.round(x)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

返回四舍五入后的整数.

[`Math.random()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

返回0到1之间的伪随机数.

所有方法看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math#Methods)。