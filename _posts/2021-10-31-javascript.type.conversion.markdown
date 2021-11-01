---
layout: post
title:  "JavaScript 类型转换"
date:   2021-10-31 15:40:00 +0800
categories: JavaScript
---

## 类型

* null
* undefined
* boolean
* number
* string
* symbol

### 装箱

当字面量直接调用其对应的对象方法的时候，称为装箱
```js
var a = 'this is string'
a.length // a自动装箱到对象String中
```

### 拆封

在需要用到对象的基本类型值的时候，称为拆封（拆箱）
```js
var a = new Stringl('This is string object');
a.valueOf() // 这个过程称为拆箱
```

## 类型转换

### ToPrimitive过程

[原文](https://262.ecma-international.org/5.1/#sec-9.1)

|类型|结果|
|---|---|
|undefined|不转换|
|null|不转换|
|boolean|不转换|
|number|不转换|
|string|不转换|
|object|首先调用[[DefaultValue]],检查该值是否有valueOf()方法,如果有并且是基本类型，就使用该值，如果没有就继续调用toString()进行强制类型转换。如果这两个方法都没有，则产生TypeError错误|

### ToString过程

[原文](https://262.ecma-international.org/5.1/#sec-9.8)

|类型|结果|
|---|---|
|undefined|`'undefined'`|
|null|`'null'`|
|boolean|`'true' or 'false'`|
|number|参见[StringToNumber过程]()|
|string|不转换|
|object|参见[ToPrimitive过程]()|

#### StringToNumber过程

[原文](https://262.ecma-international.org/5.1/#sec-9.8.1)

|数字值|返回的字符串值|
|---|---|
|`NaN`|`'NaN'`|
|`+0`or`-0`|`0`|
|负数|`'-2'`|
|Infinity|`'Infinity'`|
|一些极小或者极大的值`1.07 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000`|科学计数法`1.07e21`|


### ToNumber过程

[原文](https://262.ecma-international.org/5.1/#sec-9.3)

|类型|结果|
|---|---|
|undefined|`NaN`|
|null|+0|
|boolean|`true` is `1` or `false` is `0`|
|number|不需要转换|
|string|参见[NumberToString过程]()|
|object|参见[ToPrimitive过程]()|

#### NumberToString过程

[原文](https://262.ecma-international.org/5.1/#sec-9.3.1)

### ToBoolean过程

#### 假值

1. undefined
2. NaN 0 -0
3. null
4. false
5. ''

上述在js语言中会背判别为假，剩余的值则会判别为真

|类型|结果|
|---|---|
|undefined|`false`|
|null|`false`|
|boolean|不转换|
|number|`0 -0 NaN to false` other `true`|
|string|`'' to false` other `true`|
|object|`true`|

## 隐式类型转换

### 宽松等于类型转换过程 

1. 字符串和数字之间的相等比较 `x == y`，字符串一侧走`ToNumber`过程
2. 其他类型和布尔值之间的比较 `x == y`，布尔值一侧走`ToNumber`过程
3. 对象和非对象之间的转换 `x == y`，对象一侧走`ToPrimitive`过程
4. null 和 undefined之间的比较 `x == y`，无论null是x一侧还是y一侧，结果都为`true`

### +符号的类型转换
1. 其他类型与字符串相加的时候，其他类型都走`ToString`过程
2. 只有字符串和符号+的时候，走`ToNumber`过程

注意❗
* +'123123' -> 123123 反向不成立 '123123'+ 语法错误
* '123123'+100 -> '123123100' 反向成立 ‘100123123’

### 符号类型Symbol

1. Symbol -> Boolean 总是true
2. Symbol X-> Number
3. Symbol -> String
    * `var s1 = Symbol('cool'); new String(s1) // 'Symbol(cool)'`
    * `var s1 = Symbol('cool'); s1+''; // TypeError`
符号类型转字符串无法使用+符号进行隐式类型转换

### boolean类型的转换
1. `if()`
2. `for(;;)`
3. `while()` or `do while`
4. `? :` 三目运算
5. `||` or `&&`的结果 

### 比较关系中的类型转换 `>`,`<`,`>=`,`<=`

双方都先进行`ToPrimitive`过程，然后进行比较

```js
// 此处有一个巨大无比的坑❗❗❗❗❗❗
var a = {b:42};
var b = {b:43};
a < b // false
a == b // false
a > b // false

a <= b // true
a >=b // true
```
按照书中的逻辑，`a < b`是false，反之则是`a>=b`是true ***what fuck***