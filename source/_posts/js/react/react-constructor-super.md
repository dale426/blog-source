---
title: 理解react中的constructor与super
tags: ['react']
categories: ['JavaScript']
date: 2018-04-11 17:45:27
---

> react中的constructor与super

<!-- more -->

#### constructor( )-----super( )的基本含义

constructor( )——构造方法

　　这是ES6对类的默认方法，通过 new 命令生成对象实例时自动调用该方法。并且，该方法是类中必须有的，如果没有显示定义，则会默认添加空的constructor( )方法。

super( ) ——继承

　　在class方法中，继承是使用 extends 关键字来实现的。子类 必须 在 constructor( )调用 super( )方法，否则新建实例时会报错。

　　报错的原因是：子类是没有自己的 this 对象的，它只能继承自父类的 this 对象，然后对其进行加工，而super( )就是将父类中的this对象继承给子类的。没有 super，子类就得不到 this 对象。

#### Es5-----Es6关于继承的实现不同之处

出现上面情况的原因是，ES5的继承机制与ES6完全不同。

复习一个重要知识点——ES5中new到底做了些啥？


　`当一个构造函数前加上new的时候，背地里来做了四件事：`

　　　　`1.生成一个空的对象并将其作为 this；`

　　　　`2.将空对象的 __proto__ 指向构造函数的 prototype；`

　　　　`3.运行该构造函数；`

　　　　`4.如果构造函数没有 return 或者 return 一个返回 this 值是基本类型，则返回this；如果 return 一个引用类型，则返回这个引用类型。`

简单解释，就是在ES5的继承中，先创建子类的实例对象this，然后再将父类的方法添加到this上（ Parent.apply(this) ）。而ES6采用的是先创建父类的实例this（故要先调用 super( )方法），完后再用子类的构造函数修改this。



#### super(props)------super()-----以及不写super的区别

　　如果你用到了constructor就必须写super(),是用来初始化this的，可以绑定事件到this上;

　　如果你在constructor中要使用this.props,就必须给super加参数：super(props)；

　　（无论有没有constructor，在render中this.props都是可以使用的，这是React自动附带的；）

　　如果没用到constructor,是可以不写的；React会默认添加一个空的constructor。

