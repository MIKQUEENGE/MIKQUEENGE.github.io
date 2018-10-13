---
title: JavaScript中Array方法总览
toc: true
date: 2018-10-13 12:48:14
categories:
- Web
tags:
- JavaScript
---

## push(x)

将x添加到数组最后，可添加多个值，返回数组长度。<u>改变原数组</u>

```js
var arr = [1,2,3];
arr.push(4); // 返回4， arr变为[1, 2, 3, 4]
arr.push(5,6); // 返回6， arr变为[1, 2, 3, 4, 5, 6]
```

## unshift(x)

将x添加到数组开头，可添加多个值，返回数组长度。<u>改变原数组</u>

```js
var arr = [4,5,6];
arr.push(3); // 返回4， arr变为[3, 4, 5, 6]
arr.push(1,2); // 返回6， arr变为[1, 2, 3, 4, 5, 6]
```

## pop()

删除数组最后一个元素，返回被删除元素。<u>改变原数组</u>

## shift()

删除数组第一个元素，返回被删除元素。<u>改变原数组</u>

## join(x)

使用x将数组连接为字符串，x可以为任意对象。<u>不改变原数组</u>

## reverse()

反转数组。<u>改变原数组</u>

## slice(start,end)

返回索引start到索引end（不包含end）的新数组。<u>改变原数组</u>

## splice(index, count, value)

从索引为index处删除count个元素，插入value，value可为多个值。<u>改变原数组</u>

## sort()

对数组排序。<u>改变原数组</u>

```js
var a = [11,9,21,31]；
a.sort()；
// a为 (4) [11, 21, 31, 9]
```

sort排序是根据位来进行排序，而非值的大小，先比较第一位，数字在前，字母在后，若相同则比较后面位。

```js
a = [31, 22, 27, 1, 9]
a.sort((a, b)=>{
  return a - b
})
console.log(a)        // [1, 9, 22, 27, 31]  按数值大小正序排列
```

## toString()

将数组中的元素用逗号拼接成字符串。<u>不改变原数组</u>

## toLocaleString()

将数组中的元素使用各自的`toLocaleString()`转换后用逗号拼接成字符串。<u>不改变原数组</u>

```js
var a = [1, new Date(), 'a', {m: 1}];
var result = a.toLocaleString();
console.log(result); // '1,2018/10/3 下午9:23:59,a,[object Object]'
```



## indexOf(value)

从索引0开始查找value，如果有，返回匹配到的第一个索引，否则返回-1。<u>不改变原数组</u>

## lastIndexOf(value)

从最后开始查找value，如果有，返回匹配到的第一个索引，否则返回-1。<u>不改变原数组</u>

## concat(value)

将原数组和value拼接成新数组。<u>不改变原数组</u>

```js
var a = [1, 2], b = [3, 4], c = 5;
var result = a.concat(b, c);
console.log(result);   // [1, 2, 3, 4, 5]
console.log(a);        // [1, 2]

b = [3, [4]];
result = a.concat(b, c);
console.log(result);   // [1, 2, 3, [4], 5] concat对于嵌套数组无法拉平
console.log(a);        // [1, 2]
```

## fill(value, start, end)

使用给定value填充数组，从索引start开始end结束，不包含end。<u>改变原数组</u>

```js
var a = [1, 2, 3, 4, 5];
var result = a.fill(0, 2, 3);
console.log(result); // [1, 2, 0, 4, 5]
console.log(a); // [1, 2, 0, 4, 5]

a = [1, 2, 3, 4, 5];
console.log(a.fill(1)); // [1, 1, 1, 1, 1]  参数一个时，将该参数覆盖填充到数组每一项
a = [1, 2, 3, 4, 5];
console.log(a.fill(1, 2)); // [1, 2, 1, 1, 1]  只有start时，从索引start开始填充到数组末位
a = [1, 2, 3, 4, 5];
console.log(a.fill(1, -2)); // [1, 2, 3, 1, 1]  只有start且为负数时，从倒数|start|位开始填充到数组末位
```

## flat()

将二维数组变为一位数组，只能将第二层嵌套数组“拉平”。<u>不改变原数组</u>

## map(fn)

对数组中每一个元素执行fn函数，返回所有返回值组成的数组。<u>不改变原数组</u>

## flatMap()

相当于flat和map的结合。<u>不改变原数组</u>

## copyWithin(target, start, end)

将数组从start到end索引的元素(不包含end)复制到target开始的索引位置。<u>改变原数组</u>

```js
let a = [1, 2, 3, 4, 5]
let result = a.copyWithin(0, 3, 5)  
console.log(result) // [4, 5, 3, 4, 5]
console.log(a) // [4, 5, 3, 4, 5]  索引3到5的元素为4和5，复制到从0开始的位置，替换掉了1和2

a = [1, 2, 3, 4, 5]
console.log(a.copyWithin(2)) // [1, 2, 1, 2, 3]  参数只有一个时，start默认为0，end默认为数组长度-1
```

## entries()

返回一个新的Array迭代器对象,可用for...of遍历。<u>不改变原数组</u>

每一次next返回[index,value]

## keys()

返回一个新的Array迭代器对象,可用for...of遍历。<u>不改变原数组</u>

每一次next返回{value,done}

value实际为数组的索引：0,1,2,...

## keys()

返回一个新的Array迭代器对象,可用for...of遍历。<u>不改变原数组</u>

每一次next返回{value,done}

value实际为数组中每一项的值

## forEach()

遍历数组。<u>不改变原数组</u>

```js
var a = [1,2,3,4,5];
var result = a.forEach(function(value, index) {
  console.log(value, index);
  // 1 0
  // 2 1
  // 3 2
  // 4 3
  // 5 4
});
console.log(result);   // undefined
console.log(a);        // [1, 2, 3, 4, 5]
```

## every(fn)

判断是否数组中所有元素都满足fn，返回true或false。<u>不改变原数组</u>

## some(fn)

判断是否数组有元素满足fn，返回true或false。<u>不改变原数组</u>

## filter(fn)

返回数组中满足fn的所有元素（以新数组的形式）。<u>不改变原数组</u>

## find(fn)

返回数组中第一个满足fn函数中条件的元素值，没有则返回undefined。<u>不改变原数组</u>

## findIndex(fn)

返回数组中第一个满足fn函数中条件的元素索引，没有则返回undefined。<u>不改变原数组</u>

## includes(value)

返回一个布尔值，表示数组中是否包含value。<u>不改变原数组</u>

## reduce()

`array.reduce(function(total, currentValue, currentIndex, arr), initialValue)`

相当于一个函数累加器，接受一个回调函数的结果，然后将前一次的函数结果再和下一次的数据再次执行此回调函数。

<u>不改变原数组</u>

## reduceRight()

类似于`reduce()`，只是从数组末尾开始进行累计。<u>不改变原数组</u>

