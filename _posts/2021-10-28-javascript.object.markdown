---
layout: post
title:  "JavaScript 对象"
date:   2021-10-28 20:00:00 +0800
categories: JavaScript
---

## 对象

### 对象语法

* 文字语法 `var obj = {key:'value'}`
* 构造形式 `var obj = new Object()`

### 类型

JavaScript中的基础类型
* string
* number
* boolean
* null (某些人认为null是对象)
* undefined
* object

这些基础类型在调用一些方法的时候，例如`var str = '123';str.length`会隐式转换为下面的对应的对象类型。

* String
* Number
* Boolean
* Object
* Function
* Array
* Date
* RegExp
* Error

### 访问内容

对象可通过两种访问方式访问数据

* `obj.key // 属性访问`
* `obj[key] // 键访问`

> 其中key在对象中无论何种情况都为字符串
```js
// put
var obj = {};
obj[true] = 'foo';
obj[3] = 'bar';
obj[obj] = 'obj';
// get
obj['true']; // foo
obj['3'] // bar
obj['[object Object]'] // obj
```

### 可计算属性名

```js
var prefix = 'foo'

var obj = {
    [prefix+'bar']: 'hello world'
}

obj['foobar'] // hello world
```

### 数组

数组也是对象，你可以声明一个数组，然后向其内部添加属性，虽然可以像下面那种方式去使用，但是这不是一个好主意

```js
var arr = [1,2,3];
arr.b = 'b'
arr[0]; // 1
arr.b // 'b'
arr.length // 3
```

数组的下标如果是一个类型数字的值，会直接转换为数字
```js
var arr = [];
arr['0'] = 0;
arr[0] // 0
```

### 属性描述符
对象的属性有下面几种特性
```js
var obj = {a:1};
Object.getOwnPropertyDescriptor(obj,'a');
/**
* {
*   value: 1,
*   writable: true, // 是否可修改
*   enumerable: true, // 是否可枚举
*   configurable: true // 是否可配置
* }
*/
```

#### 可修改 (writable)

```js
// writable false
var obj = {a:1}
obj.a = 2;
obj.a // 1

// writable false
var obj = {arr:[]}
obj.arr.push(1)
obj.arr // [1]
// 即使将属性值设置为不可写，但是面对引用类型任然无计可施
```
#### 可配置 (configurable)
```js
// configurable false
// writable true
var obj = {a:1}
obj.a = 2;
obj.a // 2
Object.defineProperty(obj,'a',{
  value: 100,
  writable: false,
  configurable: true,
  enumerable: true
})
// ❌错误，无法修改，当第一次甚至可配置为false，以后就不能设置了，操作时单向的

delete obj.a
obj.a // 2
// 除了无法配置，同样也无法删除当前属性
```

#### 可枚举 (enumerable)
```js
// a enumerable false
// b enumerable true
var obj {a:1,b:2}
'b' in obj // true
for(key in obj){console.log(key)} // a
```

### [[Get]]

对象`obj`获取某个属性的顺序，如`a`属性

1. 首先会从obj.a中获取，如果存在，则返回，如果不存在则继续第二步
2. 从obj对象下的[[Prototype]]中进行查找，如果不存在则继续查找下层的[[Prototype]]，直到找到则返回`a`的值或者找不到返回`undefined`

这里面有一种情况，当返回`undefined`的时候，是`a`属性值为`undefined`还是`a`属性没找到呢？可以通过下面的方式判断
```js
var obj = {a:undefined};
obj.a // undefined
// or
var obj = {};
obj.a // undefined

// 可以通过下面两种方式进行区别
('a' in obj) // 1.true,2.false
obj.hasOwnProperty('a') // true
obj.hasOwnProperty('a') // false
```

### [[Put]]

对象`obj`赋值某属性时，过程比较复杂。

1. 如果writable = true
  1. 如何属性中存在setter，则直接调用setter
  2. 如果属性存在与`obj`中,如`var obj = {a:1}`，则直接将a赋予新的值
  3. 如果属性中不直接存在该属性，则将他赋值到obj中
2. 如果writable = false。 无论a属性上面3中情况中的哪一种，都是直接失败

### setter

### getter

