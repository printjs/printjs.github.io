---
layout: post
title:  "JavaScript 关于function中this"
date:   2021-10-24 15:00:00 +0800
categories: JavaScript
---

## JavaScript function声明的函数中this的到底指向什么?

1. demo1:
```js
var a = 1;
function A(){
    var a = 100;
    console.log(this.a);
}
A();
```
2. demo2:
```js
var a = 1;
var B {
    a: 100,
    fn:function(){
        console.log(this.a);
    }
}
B.fn();
```
3. demo3:
```js
var C {
    a: 100,
}
B.fn.apply(C);
// or 
const b = B.bind(C);
b();
```

4. demo4
```js
function A(a){
    this.a = a;
}
const c = new A();
```

### 默认指向

demo1是默认指向的例子
`this` 指向了function函数在运行时的作用域

### 隐式绑定

demo2是隐式绑定的例子
`this` 与对象`B`进行了隐式绑定，`B`对象在调用的时候`fn`指向了`B`，因为`B.fn`。但下面的方式`fn`并不会指向`B`
```js
var a = 1;
const B {
    a: 100,
    fn: function(){
        console.log(this.a);
    }
}
const b = B.fn;
b(); // 1
```
常量b是fn的引用，b执行的时候，实际上是**默认指向**了b运行时的作用域

### 显示绑定

demo3 是显示绑定的例子
在JavaScript中可以通过`apply`，`call`，`bind`等function自带的方法改变函数的this指向。

### new

`new`在其他面向对象语言中，可以实例化一个对象，但在Javascript中完全不会。具体new会做些什么，我们后面再说

如果让`new`调用一个函数，在函数**没有返回值**的情况下，`new`会创建一个对象，并且这个对象的this指向这个函数

## 绑定的优先级别

当四种情况同时发生的时候，优先级是如何的呢？

直接说结论

`new > 显示绑定 > 隐式绑定 > 默认绑定`

## 一些特殊的情况

### 被忽略的this

当使用`call`，`apply`，`bind`的时候，如果第一个参数传入`null`或者`undefined` 函数的显示绑定规则会失效，执行默认绑定规则

### 间接引用

```js
var a = 2;
function foo(){console.log(this.a)}
var A = {a:100,foo:foo};
var B = {a:200};
(B.foo = A.foo)();// 2
```

这里需要注意的是`B.foo = A.foo;B.foo() === 200`，只有上面代码在赋值引用的时候才等于2，原因是`B.foo = A.foo`返回值是函数的引用

### 箭头函数

箭头函数不执行上面的任何一种指向规则，上面的任何调用方式也改变不了箭头函数

***箭头函数中的this在定义的时候就已经确认好了，是箭头函数定义的词法作用域***
