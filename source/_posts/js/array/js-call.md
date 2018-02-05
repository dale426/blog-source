---
title: javascript之call的用法
date: 2017-11-07 12:35:00
description: "从零来理解javascript中的call的用法"
tags: [js知识点总结]
categories: [JavaScript]
---
## 正文开始
**一说到call**，

————总是**`call`**、‘张三’、‘李四’的区别什么什么的，说的很清楚，转身还是傻傻分不清楚他们的区别了，相似的事情总是喜欢一起来说，这对于新手来说总是容易混乱的，今天就来理解下call的用法；

**通俗点：**
> **`call`**的作用就是： **改变函数执行时的上下文** 也就是`this`的指向;
### 用法：
    Food.call(thisArg, arg1, arg2, ...)

### 来看个例子 --- A


```js
function fun(){
	this.name = 'fun-name'
	this.age = 'fun-age'
}
 var wrap = {
 	age: 'default',
 	name: 'default',
 	myfun: function() {
 		fun()
 	}
}

wrap.myfun();
console.log(wrap.age)   // 'default-age'
console.log(window.age)  // 'fun-age'
```
直接运行这个函数 **`wrap.myfun(); `**
> 
执行这个函数后
1、在`wrap`下面执行`myfun()`后，其中的`this`指向`window`全局的
2、在`window`全局下面创建了一个 `age`属性，值为 `'fun-age'`
3、`wrap`中的`age`还是`default`


### 来看个例子 --- B
```js
function fun(){
	this.name = 'fun-name'
	this.age = 'fun-age'
}
 var wrap = {
 	age: 'default',
 	name: 'default',
 	obj: function() {
 		fun.call(this)      // **---注意这里打 call 了---**
 	}
}

wrap.myfun();
console.log(wrap.age)   // 'fun-age'        --发生了变化---
console.log(window.age)  // 'age is not defined'   --发生了变化---
```
例子B运行时：

> `wrap.obj()`执行后，在执行`fun`时，把`this`， call进去了, 这个`this`是指向`wrap`，所以`fun`执行时其中的`this`指向的是`wrap`，自然改变的就是`wrap`中的`age`，这就是`call`的作用改变了`fun`执行时的上下文；

好累，反正我是大概懂了他（this）刚才干了什么；
***
那么在我们的coding中，一般什么时候用到call了？
## call的用法
### 利用call来 做继承
```js
var Person = function () {
    this.fun = function() { 
        console.log('longlee') }
};
var student = function () {
    Person.call(this);
};

var st = new student ();

g1.fun()  // 输出： longlee
```
如果不在student函数中执行 call，new出来的实例是没有fun属性方法的；打call就可以实现继承Person方法了；
### 判断数据的类型 
> 【object、 array、 null】
```js
var obj1 = {name: 'longlee'}
var obj2 = ['longlee']
var obj3 = null

Object.prototype.toString.call(obj1)    // "[object Object]"
Object.prototype.toString.call(obj2)    // "[object Array]"
Object.prototype.toString.call(obj3)    // "[object Null]"
Object.prototype.toString.call(12)      // "[object Number]"
....
....
 ```
 ### 类（伪）数组使用数组方法
```js
var arg = Array.prototype.slice.call(arguments);
arguments是函数接收的实际参数个数，他是一个伪数组，不具有数组的一般方法。比如 push、pop...,

但是我们能通过 Array.prototype.slice.call 转换为真正的数组
这样 arguments 就可以应用 Array 下的所有方法了。
```
### 获取数组中的最大值和最小值
```js
maxInNumbers = Math.max.call(Math, 55, 888 , 521 , -36); // 888
number 本身没有 max 方法，但是 Math 有，我们就可以借助 call 使用其方法。

```

就说到这了，再说下去，我自己也快消化不良了、、、、


***
题外话：
说到数组的最大值、最小值。我控制不住自己了，一个ES6的简洁方法
`Math.max(...[2,1,3])`  // 3
`Math.min(...[2,1,3])`  // 1

个人见解，有误之处，大神请指出，以免改正！
弄懂 call 了。可以继续打 call 了