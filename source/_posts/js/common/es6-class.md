---
title: ES6-class类与继承
tags: [class, ES6]
categories: [JavaScript]
date: 2019-03-21 19:39:11
---
tips: js继承探究之class
<!-- more -->
### class类
```js
    export class example {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        add(x, y) {
            return x+y;
        }
    }

    export class calc extends example {
        constructor(x,y) {
            super(x,y); // 子类构造函数必须使用super调用父类的构造函数;
            this.z = x+y;
        }
        mul(x,y) {
            return x*y
        }
        static div(a, b) {
            return a / b;
        }
    }
```
### 类生成实例
> import { example, calc } from '../../utils/class_example.js';

    * class example类，【假装相当于是一个构造函数】
    * 这个类【假构造函数】是有另外一个真的构造函数 constructor生成
    * 这个类【假构造函数】中的方法其实是挂载在自身prototype上的；
    * 
    * 当用new 这个类生成实例时
    * var 实例myExample = new 这个类【假的构造函数】
    * 实例myExample能够调用原型中(原型指的是构造函数的prototype对象)的方法，也就是这个类【假的构造函数】的prototype上的方法
    * example.prototype === myExample.__proto__   // true
```js
    let myExample = new example(22, 33);

    console.log('myExample', myExample);
    console.log('example.prototype', example.prototype);

    //实例的原型指向构造函数的prototype
    console.log('example.prototype === myExample.__proto__ :', example.prototype === myExample.__proto__);  // true

    //实例的constructor指向他的构造函数                                                                                                          
    console.log('myExample.constructor === example', myExample.constructor === example); // true

    //实例的constructor 指向 他的构造函数prototype上的constructor                                                                                
    console.log('myExample.constructor === example.prototype.constructor', myExample.constructor === example.prototype.constructor); //true

    // 类prototype上的构造函数 指向 的是 自身                                                                                                        
    console.log('example.prototype.constructor === example', example.prototype.constructor === example); // true                              
```
### 继承的类生成的实例
> import { example, calc } from '../../utils/class_example.js';

```js
    let myClass = new calc(10, 56);
    console.log('myClass', myClass);
    console.log( 'myClass.__proto__ === calc.prototype', myClass.__proto__ === calc.prototype) //true
    console.log('calc.__proto__ === example.prototype', calc.__proto__ === example); // true  类的继承， 它的原型指向继承的那个类，而非那个类的prototype

    <h3><b>3. 类上面的静态方法</b></h3>
    import { calc } from '../../utils/class_example.js';

    let num = calc.div(100, 20);
    console.log('num', num); // 5
  ```