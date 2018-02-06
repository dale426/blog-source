---
title: 获取浏览器url 参数
date: 2018-02-06 15:16:00
description: "获取url中参数"
tags: [算法]
categories: [JavaScript]
---

#### 1、获取当前url的方法： 
```js
当前url = 'https://www.jianshu.com/search?q=%E7%BD%91%E7%AB%99%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96&page=1&type=note'
var href = window.location.href    // 获取完整 url路径
var search = window.location.search  // 获取从？开始的参数部分

```
#### 2、将url中的参数转换成中文
 知识点 来自W3C

#####  编码`encodeURI() ` 解码`decodeURI()`
1. 把字符串作为 URI 进行编码。
2. 对 / ? : @ & = + $ #  不会转义。

##### 编码 `encodeURIComponent() `  解码 `decodeURICompnent()`
1. 把字符串作为 URI 组件进行编码。
2. 不会对 ASCII 字母和数字进行编码，
3. 也不会对这些 ASCII 标点符号 - _ . ! ~ * ' ( ) 进行编码：
4. 其他字符（比如 ：;/?:@&=+$,# 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的。

#### 3、获取URL参数
```js
url = decodeURI(search) // 编码字符 解码
var splitIndex = url.indexOf('?')  // 返回第一个？号的位置
var str = url.substring(splitIndex + 1) // 获取到查询参数

```
##### 3.1 获取url中某一个参数的值的方法
```js

var getStrParam = function (str, name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    var r = str.match(reg);
    if (r != null) return r[2];
    return '';
}
```
##### 3.2 获取url中所有的参数，序列在在一个对象中；
```js

var getAllUrlParam = function(str) {
    var urlArr = str.split('&')
    var obj = {}
    for(var i = 0; i < urlArr.length; i++) {
        var arg = urlArr[i].split('=')
        obj[arg[0]] = arg[1]
    }
    return obj
}
```
***
**一道练手题送给大家：**   
将URL中的参数序列化在一个对象中，相同的key存放在一个数组中，值为空的key，默认为true？
```js
var url = 'https://www.jianshu.com/search?q=%E5%8F%82%E6%95%B0&page=1&type=&key=aa&key=bb&'
```

预期结果：
```js
obj = {
    q: '参数',  // 中文
    page: "1",
    type: true, // 空值为ture
    key: ["aa", "bb"] // 相同的key放在数组中
}
```