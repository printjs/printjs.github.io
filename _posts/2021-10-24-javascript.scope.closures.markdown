---
layout: post
title:  "JavaScript 作用域和闭包"
date:   2021-10-24 10:00:00 +0800
categories: JavaScript
---

## JavaScript 作用域

JavaScript语言中，采用的是词法作用域

词法作用域, 还包含:
* 函数作用域 `function(){}`
* 块作用域 `{}`

> ***词法作用域***
> 词法作用域就是定义在词法阶段的作用域；词法作用域是由你写代码时将变量和块作用域写在哪里决定的，因此当词法分析器处理代码时会保持作用域不变（大部分情况）


```js
function foo(a) {
    var b = a * 2;
    function bar(c) {
        console.log(a,b,c);
    }
    bar(b * 3);
}
foo(2);
```
上述代码foo定义在了词法作用域中，bar定义在了foo函数作用域中

### JavaScript欺骗词法作用域的情况

* `eval` 函数可以接受一个字符串作为参数，并将其执行
```js
function foo(str,b) {
    eval(str);
    console.log(a,b);
} 
foo('var a = 2',3);
```

* `with` 关键字

```js
var obj = {
    a:1,
    b:2,
}
// with 子句
with(obj) {
    a = 1; // 正常
    b = 2; // 正常
    c = 3; // c属性并不存在与obj中而是在当前作用域中，词法或者函数作用域中
}
// 
```

## JavaScript 提升

在JavaScript代码中，函数与var声明的变量会被提升到当前的作用域的顶部

> ***var*** 声明的变量
> `var`声明的变量不遵守块作用域，因此在块中声明的变量会被提升到比块更外层的函数或者词法作用域当中

> ***let*** 和 ***const*** 声明的变量和常量
> `let`和`const`声明的变量和常量则会绑定在块和函数作用域中，并且不会提升

```js
let a = 1;

var c = 2;

function b(){
    // balabala
}
```
JavaScript引擎会把上面的代码解析成
```js
function b(){
    // balabala
}

var c;
let a = 1;
c = 2;
```
> ***function*** 声明的函数
> `function` 声明的函数提升是优先级最高的，会提升到最前面。在函数作用域中提升也仅会提升到函数作用域的最顶部
> `function` 切记不要在块作用域中声明函数
```js
foo(); // TypeError: foo is not a function
var a = true;
if(a){
    function foo(){console.log(true)}
}else {
    function foo(){console.log(false)}
}
```

## JavaScript 闭包

### 什么是闭包

* 无论通过任何手段将内部函数A传递到当前词法作用域范围外
* 无论在何处调用这个函数A，函数A都会获取到此前词法作用域

```js
for(var i = 0;i < 5;i++) {
    setTimeout(()=> {
        console.log(i)
    },1000);
}
// 5 次 5
// 因为var变量会进行提升，将变为全局的变量，setTimeout执行的时候，当前作用域中实际
```
```js
for(let i = 0;i < 5;i++) {
    setTimeout(()=> {
        console.log(i);
    },1000);
}
// 0,1,2,3,4,5
// let 变量会绑定在块作用域中，timeout函数执行的时候因为闭包，所以能获取到i的值
```
