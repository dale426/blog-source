---
title: 前端储存的种类与方法
tags: [js知识点总结]
categories: [前端开发]
date: 2018-09-21 14:22:11
---
摘要： localStorage、sessionStorage、cookie的用法以及区别
<!-- more -->
### localStorage的优势与用法
- localStorage拓展了cookie的4K限制
- localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的;

#### localStorage的局限

1、浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性
2、目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换
3、localStorage在浏览器的隐私模式下面是不可读取的
4、localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡
5、localStorage不能被爬虫抓取到

    localStorage与sessionStorage的唯一一点区别就是localStorage属于永久性存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空

#### localStorage的写入方法

var storage = window.localStorage
1. 方法一： 
        storage['name'] = 'dale' //写入name字段
2. 方法二： 
        storage.info = 'this is a info' // 写入info字段
3. 方法三： 
        storage.setItem("age", 18) // 写入info字段

> localStorage的使用也是遵循同源策略的，所以不同的网站直接是不能共用相同的localStorage

#### localStorage的读取方法
1. 方法一:  
        var name = storage.name 
        console.log(name); // 'dale' 
2. 方法二： 
        var info = storage['info']
        console.log(info) // 'this is a info'
3. 方法三： 
        var age = storage.getItem('age')
        console.log(age) // '18' 注意这里输出的18是String类型的

> localStorage只支持string类型的存储。

#### localStorage的删除方法

> storage.removeItem('name') // 删除key为name的键值


#### localStorage的清空方法

> storage.clear() //清除所有的storage

### sessionStorage的用法

**sessionStorage的使用方法与localStorage完全相同，唯一的区别是：`sessionStorage当会话结束的时候，sessionStorage中的键值对会被清空 `**;
详解： 
1. 当 当前tab关闭，或者浏览器关闭；sessionStorage会被清空； 
2. 复制tab标签时，sessionStorage会被复制，
3. 重新打开相同域名时tab时，sessionStorage是不会被共享的


### cookie概念和使用方法

#### cookie的基本概念
- cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下
- cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭
- 有效期设置：Max-Age
- 安全secure=true是只在ssl或者https安全协议下传输
- HttpOnly=true时，禁止通过js脚本获取cookie；防止xss攻击
- 每个域名下的cookie 的大小最大为4KB，最多20个

#### cookie的使用
##### cookie的设置

```js
    function setCookie(cname,cvalue,exdays)
        {
        var SetTime = new Date();                                         //设置过期时间
        SetTime.setTime(SetTime.getTime()+(exdays*24*60*60*1000));        //设置过期时间
        var expires = "expires="+SetTime.toGMTString();                   //设置过期时间
        document.cookie = cname + "=" + cvalue + "; " + expires;          //创建一个cookie
        }
```
##### cookie的读取
```js
    function getCookie(c_name)
    {
    if (document.cookie.length>0) 
    {
    c_start=document.cookie.indexOf(c_name + "=")
    if (c_start!=-1)
        { 
        c_start=c_start + c_name.length+1 
        c_end=document.cookie.indexOf(";",c_start)
        if (c_end==-1) c_end=document.cookie.length
        return unescape(document.cookie.substring(c_start,c_end))
        } 
    }
    return ""
    }

```
##### cookie的删除
    将cookie的有效时间改成昨天。

### 总结

#### 共同点：
> 共同点：都是保存在浏览器端、且同源的
#### 区别
1. sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；
2. localstorage在所有同源窗口中都是共享的；也就是说只要浏览器不关闭，数据仍然存在
3. cookie: 也是在所有同源窗口中都是共享的.也就是说只要浏览器不关闭，数据仍然存在


本文参考链接： [关于Cookie、session和Web Storage](https://juejin.im/post/5ad5b9116fb9a028e014fb19)