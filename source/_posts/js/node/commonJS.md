---
title: javascript模块化
tags: ['js知识点总结','node']
categories: [JavaScript]
date: 2019-03-21 17:07:27
---
tips: js模块化发展历程
<!-- more -->
### commonJS模块
> 最早出现的javascript模块化规范

#### 模块的引用
```js
var math = require('math'); 
math.add() // 100
```
#### 模块导出
在node中，上下文提供了`exports`对象用于导出当前模块的方法或者变量，在模块中存在一个 `module`对象，代表模块自身，而`exports`是`module`的一个属性；
将方法挂载在exports对象上作为属性即可定义导出的方式；
```js
// math.js
exports.add = function() {
  var params = 100
  return params;
}

exports.title = '导出标题'
```

#### 模块标识
就是传递给require的参数，比如 `require('fs'), require('./src/main.js)`

- **使用CommonJS规范的js文件调用，是一个同步的过程, 本地读取require的js后，才进行后续操作**
- **所以不适合在浏览器端执行**

### node模块

node模块分两类 

1. 核心模块：`node `提供的模块； 
2. 文件模块： 用户编写的模块；
node模块加载过后，会进行缓存，二次加载会优先从缓存检查；
***
- `node`查找文件会先在当前目录下查找 `package.json`, 通过`JSON.parse`来解析描述对象， 取出`main`属性指定的文件名进行定位， 如果没有`package.json`或者文件名错误，会默认`index`当做文件名；
- exports对象是通过形参的方式传入的， 要想实现类型class的方式引入，使用如下：
```js
module.exports = {
  add: function(){},
  title: '迂回的方式导出'
}
```

### NPM

1. 安装方法
```shell
npm install express -g
```
2. 本地包安装方法
只需要为NPM指明package.json文件所在的位置即可；
```shell
npm install <folder>
```
3. 从非官方源安装
```js
npm install underscore --registry=http://taobao.org
```
4. 查找可require的包
```js
npm ls
```

### 前后端共用模块
#### AMD规范
> AMD规范是CommonJS模块规范的一个延伸；`异步模块定义， 依赖前置， requirejs`

##### 定义模块
```js
define(id?, dependencies?, factory);
```
  id和dependencies为可填项，如果不设定id，加载这个模块时，就默认使用这个模块的文件命名，否则使用id。dependencies为当前这个模块会使用到的依赖。factory是必填项，为模块的主体内容。可以是函数，也可以是对象。如果是函数，只会被执行一次。如果是对象，则是这个模块的输出值。

##### 加载模块
```js
require([dependencies],function(){}) // require(['./index.js'],function(){})
```
require()函数接收两个参数：
1. 第一个参数是一个数组，表示所依赖的模块`【要引用的模块的路径(不带.js后缀)】`。
  * 不做任何设置的默认情况下，模块的路经查找，是以当前的文件做基础
  * 如果使用data-main属性， 模块路径查找，是以data-main指定的文件所在的路径为基础的
  * 如果使用config方法配置了baseUrl,那么路径的查找将会以baseUrl配置的路径作为基础
2. 第二个参数是一个回调函数，当前面指定的模块都加载成功后，它将被调用。加载的模块会以参数形式传入该函数，从而在回调函数内部就可以使用这些模块。

`重点：`require()函数在加载依赖的函数的时候是异步加载的，这样浏览器不会失去响应，它指定的回调函数，只有前面的模块都加载成功后，才会运行，解决了依赖性的问题。

**使用例子**
```js
//  定义
define('sayname',[],function(){
    var name = 'Byron';
    function sayName(){
        console.log(name);
    }
    return {
        sayName: sayName
    };
}());

//  使用
require(['jquery','sayname'], function($,my){
　 my.sayName(); 
});
```

#### CMD规范
定义方法
```js
defin(factory);
```
在依赖部分，CMD支持动态引入;
```js
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js')
  $('div').addClass('active');
  var timeout=require('...')  /*依赖文件所在路径*/
  timeout.init()
});

seajs.use(['myModule.js'], function(my){
  //do something
});
```
CMD是需要用到什么，才require什么，属于懒执行。AMD对待依赖的态度是预执行。
