---
title: node学习（一）
tags: ['node']
categories: ['JavaScript']
date: 2018-04-12 18:27:46
---
tips: Api写法：Error-first Callback 和 EventEmitter
<!-- more -->

##### a) 在node的回调中错误优先 
应该先定义错误的返回接收函数
- 回调函数的第一个参数返回的error对象，如果error发生了，它会作为第一个err参数返回，如果没有，一般做法是返回null。
- 回调函数的第二个参数返回的是任何成功响应的结果数据。如果结果正常，没有error发生，err会被设置为null，并在第二个参数就出返回成功结果数据。

```js
function(err, res) {
  // process the error and result
}
```
语义上讲，非空的“err”相当于程序异常；而空的“err”相当于可以正常返回结果“res”，无任何异常。

##### b) EventEmitter

事件模块是 Node.js 内置的对观察者模式“发布/订阅”（publish/subscribe）的实现，通过EventEmitter属性，提供了一个构造函数。该构造函数的实例具有 on 方法，可以用来监听指定事件，并触发回调函数。任意对象都可以发布指定事件，被 EventEmitter 实例的 on 方法监听到。

```js

var EventEmitter = require('events').EventEmitter
var life = new EventEmitter()
// 设置可以添加的事件数量
life.setMaxListeners(2)
// 添加事件
var water = function(who) {
	console.log('给' + who + '倒杯水！')
}
life.on('求安慰', water)

life.on('求安慰', function(who) {
	console.log('给' + who + '做饭！')
})
// ... 总共可以添加10个事件 【默认】
// 超出会有报出异常

// 触发事件
life.emit('求安慰', '汉子')
// 事件移除
// 将回调函数具名化 移除某一个事件
life.removeListener('求安慰', water)
// 全部移除
life.removeAllListeners()
// 移除某一个事件
life.removeListener('求安慰')

// 检测事件是否监听
var hasLifeListener = life.emit('求安慰', '')
console.log('求安慰事件是否监听:' + hasLifeListener)

// 检测事件监听的个数
console.log('life 上添加的事件数量是:' + life.listeners('求安慰').length)
console.log(EventEmitter.listenerCount(life, '求安慰'))

```
##### c) buffer

Buffer： 是一个对象， 是一个构造函数； 实例：V8引擎分配的一段内存；基本是一个数组，整数值；
```js
new Buffer(123)
//  <Buffer 31 32 33>
默认编码格式 utf-8

指定编码：
new Buffer('123', 'base64')
// <Buffer d7 6d>

长度：
var buf = new Buffer(8)   
buf.length   // 8

限制长度，写入会被截取
var buf = new Buffer(6)
buf.write('12345678')
buf // <Buffer 31 32 32 33 34 35>   6位 超出不会被缓冲

实例化数组，可以用下表读取到；
var buf = new Buffer([1,2.22,3])

buf[0]   // 1
buf[1]   // 2  [自动取证]
```

buffer转化

```js
> var buf = new Buffer('12345')
> var str = buf.toString('base64')
//> str  'MTIzNDU='

> var buf2 = new Buffer(str, 'base64')
> var str2 = buf2.toString()
// > str2  12345
```