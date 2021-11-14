---
layout: post
title:  "JavaScript Promise"
date:   2021-11-14 13:13:00 +0800
categories: JavaScript
---

## Promise

Promise 是一种反控制翻转模式，它在js中将一些不确定的因素的控制权翻转回程序可控制的范围

Promise 具有两个决议`resolve`和拒绝`reject`两个状态。开发者可以自行决定向后续的程序发送响应的决议还是，拒绝

### 具有then方法的鸭子类型

promise具有then方法，then方法有两个回调函数`fulfilled`和`rejected`。但是其他对象如果也具有then方法，那么我们认为这个对象是具有then方法的类型

### resolve

**resolve**表示决议的方式有两种
* `new Promise((resolve)=>resolve())`
* `Promise.resolve()`

表示决议的时候
1. 会返回表决值
2. 如果表决对象是一个then方法的鸭子类型，则会将then返回的值。

### reject

传入拒绝的值

### then

Promise 对象下的一个方法,有`fulfilled`和`rejected`两个回调函数

* 每次你对Promise调用then(...)，它都会创建并并返回一个新的Promise，我们可以将其链接在一起
* 不管从then(...)调用的完成回调返回值是什么，它都会被自动设置为连接Promise的完成

### catch

catch方法会捕获。但是有两种情况不会被捕获
1. promise的拒绝总会被捕获，但如果是表决中出现的错误，则catch是无法捕获的
2. 如果在then中已经进行了捕获，则catch中无法捕获

### finally

finally表示用户在表决后，总会执行。

### all

在经典的编程术语中，`门`是这样一种机制，需要等待两个或者更多并行/并发任务都完成才能继续，他们的完成顺序不重要，但是都必须完成
在Promise中，这样的模式被称之为all([...])

> 注意⚠️ 在all方法中，如果其中一个任务被拒绝，则整体会被丢弃，如果数据是空，则Promise会立即完成，请记住这一点，race有不同的行为

### race

`闩`则是等待两个或者更多并行/并发任务中，第一个返回的任务。在Promise中称之为`竞态`

> 注意⚠️在race中，如果传入的数组是空（表示没有任务），则Promise不会立即完成，永远不会表决。
