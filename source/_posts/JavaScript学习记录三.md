---
title: JavaScript学习记录三
toc: true
date: 2018-09-14 23:51:22
categories:
- Web
tags:
- JavaScript
---

——《JavaScript高级程序设计（第2版）》学习笔记

要多查阅[MDN Web 文档](https://developer.mozilla.org/zh-CN/docs/Web)

---

# 面向对象的程序设计

## 创建对象

### 工厂模式

工厂模式是软件工程领域广为人知的一种设计模式，这种模式抽象了创建具体对象的过程。

用函数来封装以特定接口创建对象的细节：

```js
function createPerson(name, age, job) {
  var o = new Object;
  o.name = name;
  o.age = age;
  o.jpb = job;
  o.sayName = function() {
    alert(this.name);
  };
  return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");

person1.sayName(); // "Nicholas"
person2.sayName(); // "Greg"
```

工厂模式虽然解决了创建多个相似对的问题，却没有解决对象识别的问题。

### 构造函数模式

我们可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.jpb = job;
  this.sayName = function() {
    alert(this.name);
  };
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.sayName(); // "Nicholas"
person2.sayName(); // "Greg"
```

构造函数模式与工厂模式的区别：

- 没有显示地创建对象(new Object())

- 直接将属性和方法赋给了this对象
- 没有return语句
- 函数名首字母大写

要创建Person的新实例，必须使用new操作符。以这种方式调用构造函数实际上会经历4个步骤：

1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（因此this指向了这个新对象）
3. 执行构造函数中的代码（为这个新对象添加属性）
4. 返回新对象

这样通过构造函数模式创建的两个对象都有一个constructor（构造函数）属性，该属性指向Person：

```js
person1.constructor == Person; // true
person1 instanceof Person; // true
person1 instanceof Object; // true， 因为所有对象均继承自Object
```

创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型，这正是构造函数模式优于工厂模式的地方。

#### 将构造函数当做函数

前边例子中的Person()函数可以通过下边任何一种方式来调用：

```js
// 当做构造函数使用
var person = new Person("Nicholas", 29, "Software Engineer");
person.sayName();

// 作为普通函数调用
Person("Greg", 27, "Doctor"); // 添加到window
window.sayName(); // "Greg"

//在另一个对象的作用域中调用
var o = new Obeject();
Person.call(o, "Kristen", 25, "Nurse");
o.sayName(); // "Kristen"
```

#### 构造函数的问题

使用构造函数的主要问题，是每个方法都要在每个实例上重新创建一遍，这是没有必要的，因此Person()可以像下边这样定义：

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.jpb = job;
  this.sayName = sayName;
}

function sayName() {
  alert(this.name);
}
```

但是这样的话，在全局作用域中定义的函数(sayName())只能被 某个对象调用，这让全局作用域有点名不副实，而且如果对象需要定义很多方法，那么就要定义很多个全局函数，这样我们自定义的引用类型就毫无封装性可言。

但是这些问题可以通过使用原型模式解决。

### 原型模式

关于prototype可以先看[这一篇](https://blog.zmj97.top/2018/09/04/JavaScript%E5%8E%9F%E5%9E%8B%E9%93%BE-prototype%E4%B8%8E-proto/)。

然后看下边这个例子：

```js
function Person() {}
Person.prototype.name = "Nicolas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
  alert(this.name);
};

var person1 = new Person();
person1.sayName(); // "Nicolas"
var person2 = new Person();
person2.sayName(); // "Nicolas"

person1.sayName == person2.sayName; // true
```

在原型模式下，对象调用这些属性和方法时，实际上是调用prototype的属性和方法。

#### 理解原型

默认情况下，所有prototype属性都会自动获得一个constructor（构造函数）属性，这个属性包含一个指向prototype所在函数的指针。

如果person1的`__proto`指向Person的`prototype`，则

```js
Person.prototype.isPrototypeOf(person); // true
```

当为对象实例添加一个属性时，这个属性就会屏蔽源性对象中保存的同名属性，但不会修改那个属性。

如果将为对象实例添加的这个属性设为null，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。

要想重新访问原型中的属性，可以使用delete操作符完全删除实例属性，

使用hasOwnProperty()可以检测一个属性是否存在于实例中（这个方法是从Object继承来的），如果是原型属性则返回false：

```js
function Person() {}
Person.prototype.name = "Nicolas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
  alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.hasOwnProperty("name"); // false

person1.name = "Greg";
person1.name; // "Greg"————来自实例
person2.name; // "Nicolas"————来自原型
person1.hasOwnProperty("name"); // true
person2.hasOwnProperty("name"); // false


delete person1.name;
person1.name; // "Nicolas"————来自原型
person1.hasOwnProperty("name"); // false

```

#### 原型与in操作符

in操作符会在通过对象能够访问给定属性时返回true，无论该属性存在于实例中还是原型中。因此对于上面的例子，在person1和person2声明后，无论何时调用`"name" in person1`或`"name" in person2`都会得到true。

因此，在hasOwnPrototype()返回false而使用in操作符返回true时，就说明这个属性是原型属性。

in操作符还可以通过for-in循环使用，返回的是所有能通过对象访问的、可枚举的（enumerated）属性和方法。

原型中不可枚举的属性和方法（即设置了[[DontEnum]]标记的属性和方法）有hasOwnProperty()、propertyIsEnumerable()、toLocalString()、toString()和valueOf()，有的浏览器也为constructor和prototype打上标记，

但是当我们在实例中添加这些属性和方法从而屏蔽了原型中的这些属性和方法时，那么这些属性和方法就会被认为是可枚举的（IE中除外）：

```js
var o = {
  toString: function() {
    return "My Object";
  }
};

for (var prop in o) {
  if (prop == "toString") {
    alert("Found toString"); // 在IE中不会显示，其他浏览器显示
  }
}
```

#### 更简单的原型方法

每添加一个属性和方法就要敲一遍Person.prototype是不必要的，同事也为了从视觉上更好地封装原型的功能，更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象：

```js
function Person() {}
Person.prototype = {
  /* 重写prototype会导致其constructor等于Object，
   * 若constructor的值很重要，可以给constructor设置回适当的值
   */
  constructor: Person,
  name: "Nicholas",
  age: 29,
  job: "Software Engineer",
  sayName: function(){
    alert(this.name);
  }
};
var person = new Person();
person.constructor == Person;
// 若是添加了上边constructor那一句则为true
```

#### 原型的动态性

由于在原型中查找值的过程是一次搜索，因此对原型对象的修改都能够立即从实例中反映出来，

但是如果像上边的例子一样重写了原型，在重写原型之前声明的实例的`__proto__`指向的仍是最初的原型：

```js
function Person() {}

var person = new Person();

Person.prototype.sayHi = function() {
  alert("hi");
};

person.sayHi(); // "hi"，没有问题

Person.prototype = {
  constructor: Person,
  name: "Nicholas",
  age: 29,
  job: "Software Engineer",
  sayName: function(){
    alert(this.name);
  }
};

person.sayHi(); // "hi"，没有问题
person.sayName(); //error
```

#### 原生对象的原型

所有原生的引用类型，都是采用原型模式创建的。因此我们亦可以对原生引用类型的prototype添加属性或方法。

以String为例：

```js
String.prototype.startsWith = function(text) {
  return this.indexOf(text) == 0;
};

var msg = "Hello World!";
msg.startsWith("Hello"); // true
```

但是不建议在产品化的程序中修改原生对象的原型。

#### 原型对象的问题

如果一个原型的属性包含引用类型值时，实例对该属性进行操作时，实际上修改的就是原型中的属性（引用类型对象名可以看做指针），因此当其他实例访问该属性时，得到的就是这个实例修改后的值：

```js
function Person() {}

Person.prototype = {
  constructor: Person,
  name: "Nicholas",
  age: 29,
  job: "Software Engineer",
  friends: ["Shelby", "Court"], // 属性值为引用类型
  sayName: function(){
    alert(this.name);
  }
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push("Van");

person1.friends; // ["Shelby", "Court", "Van"]
person2.friends; // ["Shelby", "Court", "Van"]
person1.friends == person2.friends; // true
```

### 组合使用构造函数模式和原型模式

使用构造函数模式定义实例属性，原型模式定义方法和共享的属性，

这样每个实例都会有自已的一份实例属性的副本，又共享着对方法的引用，最大限度地节省了内存，还可以向构造函数传递参数：

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ["Shelby", "Court"];
}

Person.prototype = {
  constructor: Person,
  sayName: function(){
    alert(this.name);
  }
};

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");

person1.friends; // ["Shelby", "Court", "Van"]
person2.friends; // ["Shelby", "Court"]
person1.friends == person2.friends; // false
person1.sayName == person2.sayName; // true
```

这种混合使用的模式是ECMAScript中使用最广泛、认同度最高的自定义类型的方法。可以说是一种默认模式。

### 动态原型模式

这种模式把所有信息都封装在了构造函数中，并在构造函数中通过检查某个应该存在的方法是否有效，来决定是否需要初始化模型：

```js
function Person(name, age, job) {
  // 属性
  this.name = name;
  this.age = age;
  this.job = job;
  
  // 方法
  // 只有在sayName()方法不存在时才将其添加到原型中
  // 即只有在初次调用构造函数时才会执行下面的代码
  // if语句只需要判断一个方法（例如sayName）是否存在
  if (typeof this.sayName != "function") {
    Person.prototype = {
      constructor: Person,
      sayName: function() {
      alert(this.name);
      },
      sayHi: function() {
      alert("hi");
      }
    };
  }
}
```

### 其他构造函数模式

寄生构造函数模式和[稳妥构造函数模式](https://blog.csdn.net/zqs111/article/details/50650324)，寄生构造模式没有什么意义这里就不再赘述，稳妥构造函数模式相当于为引用类型添加了private属性，有兴趣可以自行搜索。

## 继承

在ECMAScript中无法实现接口继承（与函数无法重载的理由相同，ECMAScript中的函数没有签名），

但是可以利用原型链实现实现继承。

### 原型链

除了[这一篇](https://blog.zmj97.top/2018/09/04/JavaScript%E5%8E%9F%E5%9E%8B%E9%93%BE-prototype%E4%B8%8E-proto/)讲到的，还应注意：

- 别忘记默认的原型：Object.prototype
- 确认原型和实例的关系：利用`instanceof`和`isPrototypeOf()`
- 谨慎地定义方法
  - 给原型添加方法的代码一定要放在替换原型的语句之后
  - 在通过原型链实现继承时，不同通过对象字面量创建原型方法（重写原型会切断原型链）
- 原型链的问题
  - 与原型的问题相同，如果原型包含引用类型值，那么所有同一个继承类型的实例都会共享一个引用类型值
  - 在创建子类型的实例时，不能像超类型的构造函数传递参数

### 借用构造函数

又叫伪造继承或经典继承。

在子类型构造函数得到内部利用调用超类型的构造函数，还可以传递参数。

```js
function SuperType(name) {
  this.name = name;
}
function SubType() {
  // 继承了SuperType，同时还传递了参数
  SuperType.call(this, "Nicholas");
  // 实例属性
  this.age = 29;
}

var instance = new SubType();
instance.name; // "Nicholas"
instance.age; // 29
```

但是如果方法都在构造函数中定义，函数复用就无从谈起了。

### 组合继承

combination inheritance，伪经典继承，组合使用原型链和借用构造函数。

使用原型链实现原型属性和方法的继承，通过借用构造函数实现实例属性的继承，

这样既可以实现函数复用，又能保证每个实例都有它自己的属性。

同时，`instanceof`和`isPrototypeOf`也能识别基于组合继承创建的对象。

```js
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "green", "blue"];
}

SuperType.prototype.sayName = function() {
  alert(this.name);
};

function SubType(name, age) {
  // 继承属性
  SuperType.call(this, name);
  this.age = age;
}

// 继承方法
SubType.prototype = new SuperType();

SubType.prototype.sayAge = function(){
  alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
instance1.colors; // ["red", "green", "blue", "black"]
instance1.sayName(); // "Nicholas"
instance1.sayAge(); // 29

var instance2 = new SubType("Greg", 27);
instance1.colors; // ["red", "green", "blue]
instance1.sayName(); // "Greg"
instance1.sayAge(); // 27
```

组合继承融合了前两者的优点，因此成为JavaScript中最常用的继承模式。

### 原型式继承

主要用于只是想让一个对象与另一个对象保持类似，没有必要兴师动众地创建构造函数。

```js
function object(o) {
  function F() {}
  F.prototype = o;
  return new F();
}
```

这样子实际上是object()函数对传入的对象执行了一次浅复制：

```js
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"];
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

person.friends; // ["Shelby", "Court", "Van", "Rob"]
person.name; // "Nicholas"
anotherPerson.name; // "Greg"
```

### 寄生式继承

寄生式，parasitic。

思路与寄生构造函数和工厂模式类似，创建一个仅用于封装继承过程的函数，在函数内部以某种方式来增强对象。

但是也会因为做不到函数复用而降低效率。

适用于主要考虑对象而不是自定义类型和构造函数的情况：

```js
function createAnother(original) {
  // 通过调用函数创建一个新对象，不一定使用object()函数
  var clone = object(original);
  // 以某种方式增强这个对象
  clone.sayHi = function() {
    alert("hi");
  };
  // 返回这个对象
  return clone;
}

var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"];
};

var anotherPerson = createAnother(person);
anotherPerson.sayHi(); // "hi"
```

### 寄生组合式继承

对于为什么要寄生组合式继承，看了[这篇](https://www.cnblogs.com/ghostwu/p/7440691.html)文章还有知乎上的一些回答，主要的优势是组合继承两次调用了构造函数，而寄生只使用了一次。

刚开始不理解的是，为什么在创建超类型原型副本时对超类型原型的实例化就不算调用构造函数呢？

后来仔细想了一下，的确可以不算调用了构造函数——

object()函数内的临时类型F的构造函数为空（`function F() {}`），因此可以忽略不计。

以下是代码：

```js
function object(o) {
  // 主要区别就是这里，构造函数的不同
  function F() {} 
  F.prototype = o;
  return new F();
}

function inheritPrototype(subType, superType) {
  var prototype = object(superType.prototype); // 拷贝原型
  prototype.constructor = subType; // 弥补因重写prototype而失去的默认的constructor属性
  subType.prototype = prototype; // 替换子类型原型
}

function SuperType(name) {
  this.name = name;
  this.colors = ["red", "green", "blue"];
}

SuperType.prototype.sayName = function() {
  alert(this.name);
};

function SubType(name, age) {
  // 继承属性
  SuperType.call(this, name);
  this.age = age;
}

// 寄生组合式继承
inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function(){
  alert(this.age);
};
```

# 匿名函数

没有名字的函数，也成为拉姆达(lamda)函数。

像

```js
var functionName = function(arg0, arg1, arg2) {
  // 函数体
}
```

这样的函数表达式相当于创建了一个匿名函数，然后将这个匿名函数赋给一个变量。

将函数作为参数传入另一个函数，或者从一个函数中返回另一个函数时，通常都是用匿名函数。

## 递归

（虽然不知道为什么这本书要在这里再讲一遍这个，也许可能意思是callee指向的实际上是匿名函数，不管怎么样复习一下callee吧）

前边在讲到函数内部对象arguments的属性callee(指向拥有这个arguments的函数)时有提到过递归阶乘函数这个例子：

```js
function factorial(num) {
  if (num <= 1) {
    return 1;
  } else {
    return num * arguments.callee(num-1); // 建议
    // return num * factorial(num-1);   // 不建议
  }
}

var anotherFactorial = factorial;
factorial = null;
anotherFactorial(4);
// 使用callee这里结果为24， 函数内使用factorial这里会出错
```

## 闭包

有些人会分不清**闭包**和**匿名函数**。

**闭包**指的是有权访问另一个函数作用域的函数。

创建闭包的常见方式是在一个函数内部创建另一个函数。

### 作用域链

首先先回顾一下作用域链（scope chain）。

当一个函数第一次被调用时，会创建一个执行环境（execute context）及相应的作用域链，并将作用域链赋值给一个特殊的内部属性[[Scope]]。

然后，使用this、arguments和其他命名参数的值来初始化函数的活动对象（activation object）。

这个活动对象处于作用域链的顶端，外部函数的活动对象处于第二位，外部函数的外部函数的活动对象处于第三位，... ... 直到全局执行环境的变量对象处于作用域链终点。

一般来说，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。

**但是，闭包的情况又有所不同。**

在另一个函数内部定义的函数会将外部函数的活动对象添加到它的作用域链中，当外部函数执行完毕后，如果内部的这个函数还未执行，即其作用域链还在引用外部函数的活动对象时，这个活动对象就不会被销毁。

知道内部的这个函数执行完毕，外部函数的活动对象才会随之一起销毁。

由于闭包会携带包含它的函数的作用域，因此回比其它函数占用更多内存，因此建议只有在必要时再考虑使用闭包。

### 闭包与变量

作用域链的这种配置机制有一个副作用：闭包只能取得包含函数的任何变量的最后一个值。

```js
function createFunctions() {
  var result = new Array();
  
  for (var i = 0; i < 10; i++) {
    result[i] = function() {
      return i;
    };
  }
  
  return result;
}

var funcs = createFunctions();

// 每个函数都输出10
for (var i = 0; i < funcs.length; i++) {
  document.write(funcs[i]() + "<br />");
}
```

因为每个函数的作用域链都保存着createFunctions()的活动对象，因此它们引用的都是同一个变量i，

当createFunctions()函数返回后，变量i的值为10，

所以每个函数内部的i都是10。

可以通过创建另一个匿名函数强制让闭包行为符合预期：

```js
for (var i = 0; i < 10; i++) {
    result[i] = (function(num) {
      return function(){
        return num;
      };
    })(i);
  }
```

在这里，定义了一个立即执行的匿名函数，并将它的结果赋给数组。

在立即执行时，传入了变量i，又因为函数参数是按值传递的，因此就会将i的当前值赋给num。

而这个函数内部，又创建并返回了一个访问num的闭包。

这样，result数组中每个函数都有一个自己的num变量的副本，就可以返回不同的值了。

### 关于this对象

在闭包中使用this对象也可能导致一些问题。

this对象是在运行时基于函数的运行环境绑定的：

- 在全局函数中，this等于window
- 当函数被作为某个对象的方法调用时，this等于那个对象

匿名函数的执行环境具有全局性，如果通过call()或者apply()改变环境执行环境，this会指向其他环境，但通常this指向window。

arguments也有同样的问题，

因此如果想访问作用域中的this和arguments对象，必须将对它们的引用保存到另一个闭包能够访问的变量中，然后就可以让闭包访问该对象了，以this为例：

```js
var name = "The Window";

var object = {
  name: "My Object",
  
  getNameFunc1: function() {
    return function() {
      return this.name;
    }
  },
  getNameFunc2: function() {
    var that = this;
    return function() {
      return that.name;
    }
  }
};

object.getNameFunc1(); // "The Window"
object.getNameFunc2(); // "My Object"
```

### 内存泄露

由于IE对JScript对象和COM（组件对象模型）对象**使用不同的垃圾收集例程**，因此闭包在IE中可能会导致问题。

如果闭包的作用域链中保存着一个HTML元素，那么就意味着该元素无法被销毁：

```js
function assignHansdler() {
  var element = document.getElementById("someElement");
  element.onclick = function() {
    alert(element.id);
  };
}
```

以上代码创建了一个作为element元素事件处理程序的闭包，而这个闭包又创建了一个循环引用。

由于匿名函数保存了一个对assignHandler()的活动对象的引用，因此就会导致无法减少element的引用数。

只要匿名函数存在，element的引用数至少也是1，因此它占用的内存永远都不会被回收。

可以用如下方式解决：

```js
function assignHansdler() {
  var element = document.getElementById("someElement");
  var id = element.id;
  
  element.onclick = function() {
    alert(id);
  };
  
  element = null;
}
```

这样就消除了循环引用。

需要注意的是，即使闭包不直接引用element，包含函数的活动对象中也仍然会保存一个引用。

因此 ，有必要把element设为null。

## 模仿块级作用域

JavaScript在遇到多次声明一个变量的情况时，会自动忽略后边的声明，但是会执行后边声明中的初始化。

JavaScript没有块级作用域的概念，

因此块语句中定义的变量，实际上是在包含函数中而不是语句中创建的。

可以用匿名函数来模仿块级作用域（私有作用域）来避免这个问题：

```js
(function() {
  // 块级作用域
})();
```

需要注意的是，JavaScript将function当做一个函数声明的开始，而函数声明后边是不能跟括号的。

因此上边代码中函数外面包括的括号不能省略。这样可以把函数声明转换成函数表达式。

无论在什么地方，只要临时需要一些变量，就可以使用私有作用域。

在匿名函数中的任何变量，都会在执行结束时销毁。

我们应该通过创造私有作用域来尽量少地向全局作用域添加变量和函数，以免导致命名冲突。

## 私有变量

除了前边提到的稳妥构造函数模式，还可以：

在构造函数中定义特权方法：

```js
function MyObject() {
  // 函数的私有变量
  var privateVariable = 10;
  // 函数的私有函数
  function privateFunction() {
    return false;
  }
  // 特权方法
  this.publicMethod = function() {
    privateVariable++;
    return privateFunction();
  };
}
```

在创建MyObject实例后，除了publicMethod没有任何方法可以直接访问privateVariable和privateFunction()。

或者利用私有和特权成员，隐藏那些不应该被直接修改的数据：

```js
function Person(name) {
  this.getName = function() {
    return name;
  };
  this.setName = function(value) {
    name = value;
  }
}

var person = new Person("Nicholas");
person.getName(); // "Nicholas"
person.setName("Greg");
person.getName(); // "Greg"
```

私有变量name在每一个实例的作用域中都不相同，因为每次调用构造函数都会重新创建这两个方法。

但是这样使用构造函数会有构造函数模式的缺陷：无法方法复用。每次创建实例都会创建同样一组方法，用静态私有变量来实现特权方法就可以解决这个问题。

### 静态私有变量

```js
(function() {
  var name = "";
  // 没有使用var声明，因此为全局变量
  Person = function(value) {
    name = value;
  }
  Person.prototype.getName = function() {
    return name;
  }
  Person.prototype.setName = function(value) {
    name = value;
  }
})();

var person1 = new Person("Nicholas");
person1.getName(); // "NIcholas"
person1.setName("Greg");
person1.getName(); // "Greg"

var person2 = new Person("MIchael");
person1.getName(); // "MIchael"
person2.getName(); // "MIchael"
```

在这种模式下，name就变成了静态的、由所有实例共享的属性。

因此每次改变name改变的是所有实例的name。

这样创造静态私有变量会因为使用原型而增进代码复用，但每个实例都没有自己的私有变量。

因此使用哪个方法还要视具体情况而定。

> 多查找作用域链的一个层次会一定程度上影响查找速度，这正是闭包和私有变量的一个明显的不足之处。

对于私有变量，我认为可以使用两者组合的模式，不知道对不对，这里贴出想法，欢迎指正（zmj原创，转载需注明出处）：

```js
function Person(name) {
  this.getName = function() {
    return name;
  };
  this.setName = function(value) {
    name = value;
  }
}

(function() {
  var teacher = "Nicholas"; // 初始化
  Person.prototype.getTeacher = function() {
    return teacher;
  }
  Person.prototype.setTeacher = function(value) {
    teacher = value;
  }
})();
```

这样，就既有实例自己的私有变量，也有静态私有变量了。

### 模块模式

模块模式（module pattern）是为单例（singleton）创建私有变量和私有方法。

所谓单例就是只有一个实例的对象，一般以对象字面量的方式来创建：

```js
var singleton = {
  name: value,
  method: function() {
    // 这里是方法的代码
  }
};
```

模块模式通过为单例添加私有变量和特权方法来使其增强：

```js
var singleton = function() {
  // 私有变量和私有函数
  var privateVariable = 10;
  function privateFunction() {
    return false;
  }
  
  // 特权/公有方法和属性
  return {
    publicProperty: true,
    publicMethod: function() {
      privateVariable++;
      return privateFunction();
    }
  };
}();
```

这种模式在需要对单例进行某些初始化，同时又需要维护其私有变量时是十分有用的：

```js
function BaseComponent() {}
function OtherComponent() {}

var application = function() {
  // 私有变量和函数
  var components = new Array();
  // 初始化
  components.push(new BaseComponent());
  // 公共
  return {
    getComponentCount: function() {
      return components.length;
    },
    registerComponent: function(component) {
      if (typeof component == "object") {
        components.push(component);
      }
    }
  };
}();

application.registerComponent(new OtherComponent());
application.getComponentCount(); // 2
```

在Web应用程序中，经常使用一个单例来管理应用程序级的信息。

以这种模式创建的单例都是Object的实例。

### 增强的模块模式

如果单例必须是某种类型的实例，还必须添加某些属性和/或方法加以增强，可以使用增强的模块模式：

```js
function BaseComponent() {}

var application = function() {
  // 私有变量和函数
  var components = new Array();
  // 初始化
  components.push(new BaseComponent());
  // 创造application的一个局部副本
  var app = new BaseComponent();
  // 公共接口
  app.getComponentCount: function() {
    return components.length;
  }；
  app.registerComponent: function(component) {
    if (typeof component == "object") {
      components.push(component);
    }
  }；
  // 返回这个副本
  return app;
}();
```

