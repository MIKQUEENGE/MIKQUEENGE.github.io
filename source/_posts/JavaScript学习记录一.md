---
title: JavaScript学习记录一
toc: true
date: 2018-09-11 18:26:52
categories:
- Web
tags:
- JavaScript

---

——《JavaScript高级程序设计（第2版）》学习笔记

------

# 简介

JavaScript是一种专门为网页交互而设计的脚本语言，由以下三个不同的部分组成：

- ECMAScript（发音 ek-ma-script，伪语言），由ECMA-262定义，提供核心语言功能；
- 文档对象模型（DOM，Document Object Model），提供访问和操作网页内容的方法和接口；
- 浏览器对象模型（BOM，Browser Object Model），提供与浏览器交互的方法和接口；

在HTML中使用JavaScript：

- 现代Web应用一般都把全部JavaScript引用放在`<body>`元素中，放在页面内容的后边来最后加载JavaScript代码（虽然defer属性可以实现同样的效果，但是不是所有浏览器都支持这个属性，因此一般不使用这个属性）；

# 基本概念

## 语法

1. 区分大小写
2. <b>标识符</b>，指变量、函数、属性的名字，或函数的参数，需要满足：
   1. 第一个字符必须是字母、下划线(_)或者一个美元符号($)
   2. 其他字符可以是数字、字母、下划线、美元符号
   3. 按照惯例，采用**驼峰**大小写格式，即第一个字母小写，剩下的每个有意义的单词的首字母大写
   4. 不能把**关键字、保留字、true、false、null**用作标识符
3. 注释，跟C++相同
4. 以分号结尾，可以不加分号，但是不推荐

## 关键字和保留字

- 关键字：break、case、catch、continue、default、delete、do、else、finally、for、function、if、in、instanceof、new、return、switch、this、throw、try、typeof、var、void、while、with
- 保留字： abstract、boolean、byte、char、class、const、debugger、double、enum、export、extends、final、float、goto、implements、import、int、interface、long、native、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile

## 变量

- ECMAScript的变量是松散类型（弱类型）的，无需明确的类型声明，可以用来保存任何类型的数据。
- 未初始化的变量值为`undefined`
- 使用var操作符定义的变量将成为定义该变量的作用域中举局部变量
- 不使用var操作符定义的变量是全局变量（不推荐）

## 数据类型

ECMAScript有五种简单（基本/原始）数据类型：

- Undefined
- Null
- Boolean
- Number
- String

和一种复杂数据类型：

- Object

因为ECMAScript数据类型具有动态性，因此没有再定义其他数据类型的必要。

### typeof操作符

因为typeof是操作符，因此括号不是必需的，也就是可以`typeof 126`也可以`typeof(126)`。

对一个值使用typeof操作符可能返回下列某个字符串：

- undefined
- boolean
- string
- number
- object
- function

### Undefined类型

只有一个值——undefined。

虽然对声明未初始化的变量（假设为a）和未声明的变量（假设为b）使用typeof均返回undefined，但是对他们使用alert会得到不同的结果：

对于a : 会弹出undefined

对于b : 会返回错误Uncaught ReferenceError: b is not defined

因此建议在声明时就进行初始化，以此来分别a和b。

### Null类型

只有一个值——null。

从逻辑角度来看，null值表示一个空对象指针，因此：

```javascript
var a = null;
alert(typeof a); // "object"
```

因为undefined派生自null，因此ECMA-262规定`undefined == null`为true。

只要意在保存对象的变量还没有真正的保存对象，都应该让它保存为null。

### Boolean类型

只有两个值——true和false。

转换为Boolean的转换规则：

| 数据类型  | 转换为true的值          | 转换为false的值 |
| --------- | ----------------------- | --------------- |
| Boolean   | true                    | false           |
| Stirng    | 任何非空字符串          | “”（空字符串）  |
| Number    | 任何非0值（包括无穷大） | 0和NaN          |
| Object    | 任何对象                | null            |
| Undefined | n/a（不适用）           | undefined       |

### Number类型

使用IEEE754格式表示整数和浮点数。

- 八进制数以`0`开头如`070`表示八进制的56
- 十六进制数以`0x`开头如`0xA`或`0xa`表示十六进制的10
- 浮点数必须包含一个小数点，小数点前若为0则可以省略，但不推荐这么做
- 若小数点后没有跟任何数字如`1.`或本身就是一个整数如`1.0`则会被保存为整数（因为保存浮点数值需要的内存空间是整数值的两倍）
- 可以用科学计数法表示浮点数：例如使用`1.26e-9`表示1.26乘以10的-9次方
- 浮点数最高精度为17位小数，但计算精度不如整数，例如0.1加0.2的计算结果不为0.3
- Number.MAX_VALUE = 1.7976931348623157e+308, 超出值转换为Infinity
- Number.MIN_VALUE = 5e-324， 超出值转换为-Infinity
- 可以使用isFinite()函数判断一个数是否有穷
- NaN, Not a Number，非数值，如任何数值除以0都会得到NaN
- NaN与任何值都不相等，`NaN == NaN`返回false，因此可以用函数isNaN()判断是否为NaN，true和false可以转换为数值，因此都不是NaN，字符串无法转换为数值，因此是NaN
- Number()可以将任何数据类型的非数值转换为数值，parseInt()和parseFloat()用于把字符串转换为数值

Number()转换规则：

- true和false分别转换为1和0
- 数值只是简单的传入传出
- null返回0
- undefined返回NaN
- 字符串遵循以下规则：
  - 若字符串只包含数字，则转换为十进制数值，忽略前置0
  - 若字符串为有效的浮点格式，如“1.1”，不包含数字、小数点、负号以外的字符，则转换为对应的浮点数，忽略前置0
  - 若字符串为有效的十六进制格式，如“0xf”，除了其他字符外还不可以有负号，转换为相同大小的十进制整数值
  - 若字符串是空的，则返回0
  - 其他格式返回NaN

对于parseInt()和parseFloat()：

- 关于前置0，我在Chrome里试了一下：
  - parseInt("070")返回的是70，并不会看做八进制数
  - parseInt(070)返回的是56，看做八进制数
- 有效的格式如有效的浮点格式指浮点数处于字符串开头，若字符串开头既不是数字也不是负号则不是有效格式，若以小数点开头，小数点跟有数字则是有效的浮点格式，小数点后跟的不是数字则不是有效的格式
- 可以指定基数（进制）为第二个参数

### String类型

由零或多个16位Unicode字符组成的字符序列，即字符串。

- 字符串用双引号或单引号表示都是有效且没有区别的，但左右引号必须匹配。
- 字符串a的长度可以用它的属性`length`即`a.length`来获得。
- JavaScript字符以UTF-16存储，即每个字符要么存储为2个字节，要么存储为4个字节（每个字节8位）；二String的length属性返回的是字符串中两个字节字符的数目，也就是说对于一个4个字节的字符，length会把它当做两个字符。
- 字符串不能修改，只能销毁再填充。
- 数的toString()可以传入基数（进制）为参数。
- null和undefined没有toString()方法，但可以使用String()方法。

字符字面量（转义序列）：

| 字面量  | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| \n      | 换行                                                         |
| \t      | 制表                                                         |
| \b      | 空格                                                         |
| \r      | 回车                                                         |
| \f      | 进纸                                                         |
| \\\     | 斜杠                                                         |
| \\'     | 单引号                                                       |
| \\"     | 双引号                                                       |
| \\xnn   | 以16进制代码nn表示的一个字符（其中n为0-F），例如\\x41表示“A”。 |
| \\unnnn | 以16进制代码nnnn表示的一个Unicode字符（其中n为0-F），\\u03a3表示希腊字符∑ |

### Object类型

对象是一组数据和功能的集合。具体的后边的章节会学到，这里就不展开了。

## 操作符

### 一元操作符

- ++、--对整数、浮点数、字符串、布尔值、对象都适用
  - 对非数值，先使用Number()将其转换为数值再进行加减1
- +、- 同样对整数、浮点数、字符串、布尔值、对象都适用，规则和++、--的差不多

### 位操作符

先将64位的数值转换为32位数值，然后进行位操作，最后再转换为64位数值：

- 对undefined、null、NaN和Infinity应用位操作这两者会被当成0来处理
- 对非数值，先使用Number()将其转换为数值再应用位操作
- 按位非(NOT) : `~`，返回反码，等于原值的负数减1
- 按位与(AND) : `&`
- 按位或(OR) : `|`
- 按位异或(XOR) : `^`，异或指操作的两个位均为1或均为0则返回0，一个是1一个是0则返回1
- 左移 : `<<`，左移不会影响符号位，左移出现的空位用0填充
- 有符号右移 : `>>`，用符号位的值来填充右移出现的空位
- 无符号右移 : `>>>`，所有32位都向右移动（包括符号位），并用0填充右移出现的空位

### 布尔操作符

- 逻辑非：`!`，先将操作数转换为布尔值，然后求反
  - 对一个对象进行逻辑非操作，返回false
  - 对一个空字符串进行逻辑非操作，返回true
  - 对非空字符串进行逻辑非操作，返回false
  - 对0、null、NaN、undefined进行逻辑非操作，返回true
  - 对任意非0数值包括Infinity进行逻辑非操作，返回false
  - 同时使用两个逻辑非操作符`!!`，实际上模拟了Boolean()转型函数的行为，0、空字符串、null、NaN、undefined返回false
- 逻辑与：`&&`，逻辑与操作不一定返回布尔值
  - 如果有一个操作数是null，则返回null
  - 如果有一个操作数是NaN，则返回NaN
  - 如果有一个操作数是undefined，则返回undefined
  - 如果第一个操作数是对象或者两个都是对象，返回第二个操作数
  - 如果第二个操作数是对象，则返回第一个操作数
  - 如果第一个操作数为false，则返回false
- 逻辑或：`||`
  - 如果两个操作数都是null，则返回null
  - 如果两个操作数都是NaN，则返回NaN
  - 如果两个操作数都是undefined，则返回undefined
  - 如果第一个操作数是对象或者两个都是对象，返回第一个操作数
  - 如果第一个操作数求值结果为false，则返回第二个操作数
  - 如果第一个操作数求值结果为true，则返回第一个操作数

### 乘性操作符

若操作数不是数值，则后台会先使用Number()将其转换为数值再进行计算

- 乘法：`*`
  - 若结果超出ECMAScript数值的表示范围，则返回Infinity或-Infinity
  - 若一个操作数为NaN，则结果为NaN
  - Infinity * 0 等于 NaN
  - Infinity与非0值相乘，结果还是Infinity或-Infinity
- 除法：`/`
  - 若结果超出ECMAScript数值的表示范围，则返回Infinity或-Infinity
  - 若一个操作数为NaN，则结果为NaN
  - Infinity / Infinity 等于 NaN
  - Infinity被任意数值除，结果仍为Infinity
  - 0 / 0 等于 NaN
  - 非零数（包括Infinity）除以0结果为Infinity或-Infinity
- 求模：`%`
  - Infinity % 有限值 的结果为NaN
  - 任意值 % 0 的结果为NaN
  - Infinity % Infinity 的结果为NaN
  - 有限大值a % Infinity 的结果为a
  - 0 % 任意值 的结果为0

### 加性操作符

- 加法：`+`
  - 如果有一个操作符为NaN，则结果为NaN
  - Infinity加-Infinity，结果为NaN
  - +0加+0为+0，-0加-0结果为-0
  - +0加-0结果为+0
  - 如果两个操作数都为字符串，则拼接起来
  - 若有一个操作数不是数值，则将两个都转换（toString()）为字符串，然后两个字符串拼接起来
- 减法：`-`
  - 如果有一个操作符为NaN，则结果为NaN
  - Infinity减Infinity或者-Infinity减-Infinity结果为NaN
  - +0减+0为+0，+0减-0为-0，-0减-0为+0
  - 如果有操作数为字符串、布尔值、null、undefined，则先调用Number()函数将其转换为数值，若有一个转换后为NaN，则结果为NaN
  - 如果有操作数为对象，则调用其valueOf()方法获取其数值，若值为NaN，则结果为NaN，若没有valueOf()方法，则调用其toString()方法并将得到的字符串转化为数值

### 关系操作符

`<`、`>`、`<=`、`>=`

- 若两个操作数都是数值则进行数值比较
- 若两个操作数都是字符串，则比较两个字符串的字符编码值
- 如果一个是数值，则将另一个操作数转换为一个数值，然后执行数值比较
- 如果一个操作数是对象，则先用valueOf()换取数值进行比较，若对象没有valueOf()方法，则调用toString()，然后转换为数值进行比较
- 如果一个操作数是布尔值，则将其转换为数值然后进行比较
- 如果一个操作数是NaN，则返回false

### 相等操作符

#### 相等和不相等

`==`、`!=`，先转换再比较

- 如果一个操作数为布尔值，则将其转换为数值进行比较
- 如果一个操作数是字符串，一个操作符是数值，则将字符串转换成数值再进行比较
- 如果一个操作数是对象而另一个不是，则调用对象的valueOf()方法，用得到的数值按照前边的规则进行比较
- null和undefined是相等的
- 在比较相等性之前，不能将null和undefined转换成其他任何值
- NaN和任何值（包括NaN）都不相等
- 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则`==`返回true，`!=`返回false

特殊情况结果：

```javascript
null == undefined // true
"NaN" == NaN // false
5 == NaN // fale
NaN == NaN // false
NaN != NaN // true
false == 0 // true
true == 1 // true
true == 2 // false
undefined == 0 // false
null == 0 // false
"5" == 5 // true
```

#### 全等和不全等

`===`、`!==`，仅比较不转换，首先类型相同然后值相同

### 条件操作符

`variable = boolean_expression ? true_value : false_value`

### 赋值操作符

`=`

复合操作符： `*=`,`/=`,`%=`,`+=`,`-=`,`<<=`,`>>=`,`>>>=`

例如复合操作符加等于`+=`：

```javascript
var num = 10;
num += 5;
```

相当于：

```javascript
var num = 10;
num = num + 5;
```

### 逗号操作符

声明多个变量、一条语句中执行多个操作用逗号隔开；

在赋值时，逗号操作符总会返回表达式中的最后一项：

```javascript
var num = (1, 2, 6, 5, 3, 0); // num的值为0
```

## 语句

### if、while、for语句

即使要执行的只有一句代码也要用`{}`括起来；

关于循环的块作用域后边的章节会有讲解。

### do-while语句

在对条件表达式求值之前，循环体内的代码至少会执行一次，这种后测试语句最常用语循环体内的代码至少要执行一次的情形。

### for-in语句

`for (property in expression) statement`

例如：

```javascript
for (var propName in window) {
    console.log(propName);
}
```

### label、break、continue语句

label语句的语法为`label: statement`，可以由`break`和`continue`语句引用，常用与和循环语句配合使用。

例子可以看[这里](https://blog.csdn.net/x386277405/article/details/78673757)

### with语句

**由于大量使用with语句会导致性能下降，还会给调试代码造成困难，因此开发大型应用程序时不建议使用with语句。**

`with (expression) statement`，将代码的作用于设置到一个特定的对象中。

例如

```javascript
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;
```

等价于

```javascript
with (location) {
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}
```

### switch语句

- switch语句中可以使用任何数据类型
- 每个case的值不一定是常量，可以是变量甚至是表达式
- switch在比较值时使用的是全等操作符，因此不会发生类型转换（例如“10”不等于10）

## 函数

function functionName(arg0, arg1, ..., argN) {

```
statements
```

}

返回值不是必需的。

关于参数可以看[这篇文章](https://www.cnblogs.com/hanhanhan/p/5765920.html)中的例子来理解。需要注意的是：没有传递值的命名参数自动赋予undefined值，关于引用传递可以看[这里](https://www.cnblogs.com/refe/p/5101744.html)。

因为ECMAScript函数没有签名（因为其参数是由包含零或多个值的数组来表示的），因此不能像传统意义上那样实现重载。但可以通过检查传入函数的参数类型和数量进行不同的操作来模拟重载。