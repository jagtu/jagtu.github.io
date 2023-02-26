---
layout: post
title: ES6中的Javascript了解
categories: 前端
description: Javascript基础知识了解。
keywords: Javascript
---

## ECMAScript 6 是什么？

ECMAScript 6 也称为 ES6 和 ECMAScript 2015。

一些人把它称作 JavaScript 6。

本章介绍 ES6 中的一些新特性。

### 1、JavaScript 基础

#### let

let 语句允许您使用块作用域声明变量。

```javascript
var x = 10;
// Here x is 10
{ 
  let x = 2;
  // Here x is 2
}
// Here x is 10
```

#### const

`const` 语句允许您声明常量（具有常量值的 JavaScript 变量）。

常量类似于 `let` 变量，但不能更改值。

```javascript
var x = 10;
// Here x is 10
{ 
  const x = 2;
  // Here x is 2
}
// Here x is 10
```

#### 指数运算符

取幂运算符（`**`）将第一个操作数提升到第二个操作数的幂。

```js
var x = 5;
var z = x ** 2;          // 结果是 25
```

`x ** y` 的结果与 `Math.pow(x,y)` 相同：



#### 默认参数值

`ES6` 允许函数参数具有默认值。

```javascript
function myFunction(x, y = 10) {
  // y is 10 if not passed or undefined
  return x + y;
}
myFunction(5); // 将返回 15
```

#### Array.find()

`find()` 方法返回通过测试函数的第一个数组元素的值。

此例查找（返回）第一个大于 18 的元素（的值）：

```js
var numbers = [4, 9, 16, 25, 29];
var first = numbers.find(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

#### Array.findIndex()

`findIndex()` 方法返回通过测试函数的第一个数组元素的索引。

此例确定大于 18 的第一个元素的索引：

```js
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

#### 新的数字属性

ES6 将以下属性添加到 Number 对象：

- EPSILON
- MIN_SAFE_INTEGER
- MAX_SAFE_INTEGER

```js
var x = Number.EPSILON;
var x = Number.MIN_SAFE_INTEGER;
var x = Number.MAX_SAFE_INTEGER;
```

#### 新的数字方法

ES6 为 Number 对象添加了 2 个新方法：

- Number.isInteger()
- Number.isSafeInteger()

#### Number.isInteger() 方法

如果参数是整数，则 `Number.isInteger()` 方法返回 `true`。



```js
Number.isInteger(10);        // 返回 true
Number.isInteger(10.5);      // 返回 false
```


#### Number.isSafeInteger() 方法

安全整数是可以精确表示为双精度数的整数。

如果参数是安全整数，则 `Number.isSafeInteger()` 方法返回 `true`。

```js
Number.isSafeInteger(10);    // 返回 true
Number.isSafeInteger(12345678901234567890);  // 返回 false
```
安全整数指的是从 -(253 - 1) 到 +(253 - 1) 的所有整数。

这是安全的：9007199254740991。这是不安全的：9007199254740992。

#### 新的全局方法

ES6 还增加了 2 个新的全局数字方法：

- isFinite()
- isNaN()

#### isFinite() 方法

如果参数为 `Infinity` 或 `NaN`，则全局 `isFinite()` 方法返回 false。

否则返回 true：



```js
isFinite(10/0);       // 返回 false
isFinite(10/1);       // 返回 true
```

#### isNaN() 方法

如果参数是 `NaN`，则全局 `isNaN()` 方法返回 `true`。否则返回 `false`：



```js
isNaN("Hello");       // 返回 true
```


#### 箭头函数（Arrow Function）

箭头函数允许使用简短的语法来编写函数表达式。

您不需要 `function` 关键字、`return` 关键字以及*花括号*。



```js
// ES5
var x = function(x, y) {
   return x * y;
}

// ES6
const x = (x, y) => x * y;
```


箭头功能没有自己的 `this`。它们不适合定义*对象方法*。

箭头功能未被提升。它们必须在使用*前*进行定义。

使用 `const` 比使用 `var` 更安全，因为函数表达式始终是常量值。

如果函数是单个语句，则只能省略 `return` 关键字和花括号。因此，保留它们可能是一个好习惯：



```js
const x = (x, y) => { return x * y };
```

### 2、ES6中的Javascript解构

ES6中的解构特性能让我们从对象（Object）或者是数组（Array）中取值的时候更方便，同时写出来的代码在可读性方面也更强。之前接触过python语言的小伙伴应该对这个不会陌生，这个特性早已在python中实现了。在python中，我们可以通过下面的代码来取值

```js
lst = [3, 5]
first, second = lst 
print(first, second)
```

first和second两个变量，分别被赋值上了数组中的3和5，是不是很简单很清晰？

那在有这个特性之前，我们一般怎么从对象或数组中取值呢？来看看下面的代码：

```js
let list = [3, 5]
let first = list[0]
let second = list[1]
```

在这种方式中，你必须得手动指定个数组下标，才能把对应的值赋给你指定的变量。那如果用ES6的解构特性，代码将会变得更简洁，可读性也更高：

```js
let [first, second] = list;
```

#### 对象的解构

##### 基础对象解构

首先我们来看看ES6中基本的对象解构应该怎么写：

```js
const family = {
	father: ''
	mother: ''
}
const { father, mother } = family;
```

我们从family对象中解构出来了它的两个属性father和mother，并赋值给了另外定义的father和mother对象。此后，我们就能直接通过father和mother变量获取到family中相应key的值了。这个例子是解构对象最简单的一种应用，下面来看看更有趣的。

#### 解构没有声明过的对象

在上面的例子中，我们先声明的family对象，然后再通过解构语法获取其中的值。那如果不声明是否可以呢：

```js
const { father, mother } =  {father: '',mother: ''}
```

其实也是可以的，在一些情况下，我们是没有必要特意去声明一个对象或是把对象赋值给一个变量，然后去才解构的。很多时候我们可以直接解构未声明的对象。

#### 解构对象并重命名变量

我们也可以将对象中的属性解构之后，并对其重新命名，比如：

```js
const { father: f, mother:m } =  {father: '1',mother: '2'}
console.log(f); // '1'
```

在上面的代码中，对象中的father被解构出来后，重新赋值给了变量f，与前一个例子相比，相当于重名了了father变量为f。接下来就可以用f继续进行操作。

#### 解构默认值

想象一下一种场景，后台返回了一个family对象，原本family对象约定了有三个属性，分别为father，mother，child。你拿到返回的数据并解构这三个属性：

```js
const { father, mother, child } =  {father: '1',mother: '2', child: '3'}
```

这看上去没有什么问题，但现实情况总是不如人意。后台由于代码有bug，返回的family对象中，只包含了mother和child，漏了father。这时经过上面代码的解构后， father就会变成undefined：

```js
const { father, mother, child } =  {father: '1',mother: '2'}
console.log(child) //undefined
```

很多时候我们会想要在后台漏了某个属性的时候，也能解构出一个默认值。那其实可以这么写：

```js
const { father = '1', mother = '2', child = '3'} =  {father: '1',mother: '2'}
console.log(child) //'3'
```

结合前一个例子，你既可以改变量名又能赋值默认值：

```js
const { father: f = '1', mother: m = '2', child: c = '3'} =  {father: '1',mother: '2'}
```

#### 在函数参数中解构

```text
const family = {
	father: ''
	mother: ''
}
function log({father}){
	console.log(father)
}
log(family)
```

在函数的参数中，运用解构的方式，可以直接获取出入对象的属性值，不需要像以往使用family.father传入。

#### 解构嵌套对象

在上面的例子中，family的属性都只有1层，如果family的某些属性的值也是一个对象或数组，那怎么将这些嵌套对象的属性值解构出来呢？来看看下面的代码：

```text
const family = {
	father: 'mike'
	mother: [{
		name: 'mary'
	}]
}
const { father, mother: [{
	name:motherName
}]} = family;

console.log(father); //'mike'
console.log(motherName) //'mary'
```

#### 数组的解构

数组的解构方式其实跟对象的非常相似，在文章开头也略有提及，不过我们还是来看一下数组解构的一些典型场景。

#### 基础对象解构

```text
const car = ['AUDI', 'BMW'];

const [audi, bmw] = car;
console.log(audi); // "AUDI"
console.log(bmw); // "BMW"
```

只要对应数组的位置，就能正确的解构出相应的值。

#### 解构默认值

同对象解构的默认值场景，很多时候我们也需要在解构数组的时候加上默认值以满足业务需要。

```text
const car = ['AUDI', 'BMW'];

const [audi, bmw, benz = 'BENZ'] = car;
console.log(benz); // "BENZ"
```

#### 在解构中交换变量

假设我们有如下两个变量：

```text
let car1 = 'bmw';
let car2 = 'audi'
```

如果我们想交换这两个变量，以往的做法是:

```text
let temp = car1;
car1 = car2;
car2 = temp;
```

需要借助一个中间变量来实现。那利用数组的解构，就简单很多：

```text
let car1 = 'bmw';
let car2 = 'audi'
[car2, car1] = [car1, car2]
console.log(car1); // 'audi'
console.log(car2); // 'bmw'
```

如果是想在一个数组内部完成元素位置的交换，比如吧[1,2,3]交换成[1,3,2]，那么可以这么实现：

```text
const arr = [1,2,3];
[arr[2], arr[1]] = [arr[1], arr[2]];
console.log(arr); // [1,3,2]
```

#### 从函数的返回解构数组

很多函数会返回数组类型的结果，通过数组解构可以直接拿值：

```text
functin fun(){
	return [1,2,3]
}
let a, b, c; 
[a, b, c] = fun();
```

当然，如果我们只想要函数返回数组中的其中一些值，那也可以把他们忽略掉

```text
functin fun(){
	return [1,2,3]
}
let a, c; 
[a, , c] = fun();
```

可以看到，ES6的解构特性在很多场景下是非常有用的。期望大家能更多的将解构运用到项目中，让代码变得更加简单，清晰易懂。



### 3、扩展运算符

 `...`叫扩展运算符，是在ES6中新增加的内容，它可以在函数调用/数组构造时，将数组表达式或者string在语法层面展开；

还可以在构造字面量对象时将对象表达式按照key-value的方式展开。

说白了就是把衣服脱了，不管是大括号（[]）、花括号（{}）、字符串（''），统统不在话下，**全部脱掉脱掉！**
