---
title: 前后端联调工具之-Fiddler
tags: ['联调工具']
categories: ['前端开发']
date: 2018-09-01 12:55:49
---
tips: 开发必备调试工具-fiddler
<!-- more -->

### 前言
>  现在的大型项目基本前后端分离；前端不仅仅只是负责界面，交互了；需要做的工作越来越多，但是都离不开和各个部门的沟通协作，这样才能高效率；最为密切的就是后端了，今天就简单的分享下在项目实战过程中用到的利器--`fiddle`工具；

不就是fiddle嘛，抓包工具嘛，NO,NO,NO......

Fiddler是最强大最好用的Web调试工具之一，它能记录所有客户端和服务器的http和https请求，允许你监视，设置断点，甚至修改输入输出数据，Fiddler包含了一个强大的基于事件脚本的子系统，并且能使用.net语言进行扩展

你对HTTP 协议越了解， 你就能越掌握Fiddler的使用方法. 你越使用Fiddler,就越能帮助你了解HTTP协议.
***
### 轻描淡写-介绍下fiddle
先贴一个官网地址：[fiddle官网](http://www.telerik.com/fiddler)
至于怎么形容他： **The free web debugging proxy for any browser, system or platform**
抓包、调试、代理、各种很强大的功能
1. 不费话了，开始干活
下载，一路next就可以安装好；ok，将看到这个界面

![](https://user-gold-cdn.xitu.io/2017/9/19/3124242747bc180d5bd04f6154b4de88)
左边列表就是捕获到的所有请求，不妨随便百度一下，就能看到你百度的请求；
> 这个是我点击百度搜索框，触发的请求，右边`webform`中可以看到这个请求的内容，下边的`JSON`是服务器返回的内容；很清晰，一目了然。

fiddler工具的基本使用可以参考官网文档或者慕课网有视频教程；
### fiddler的厉害之处
#### To 测试妹子
对于测试来说，如上所示，分析请求，定位是前端还是后端的问题，检验界面与数据是否一致，有时候出现神奇问题，要么是逻辑问题，要么是界面和数据不一致造成的，精准定位问题，精准发锅；
#### To 大前端
fiddler不只是具有简单的抓包功能，在他的社区有很多的插件，使他的能力发挥到极致；
详细介绍下项目中常常使用的一个神器 **Willow**`--Fiddler的插件，提供重定向和host主机等功能`,三秒钟解决联调、跨域问题，没错就是三秒.
贴一个下载地址：
[fiddler2 + willow 下载](链接：http://pan.baidu.com/s/1boJxtqn)  密码：o3lg
[fiddler4 + willow 下载](链接：http://pan.baidu.com/s/1hsaihog ) 密码：kf8p
安装`fiddler`后安装插件 `willow`,willow如果安装失败，请尝试右键使用 管理员身份打开；安装好后会在右边看到一个绿色的小图标，ok，点击他；

![](https://user-gold-cdn.xitu.io/2017/9/19/7b53459a216d4ce5a1ef0943d0a31174)

核心内容
***
**问题现状：** 
我们的项目通常开发时都跑在本地服务下，一般是`127.0.0.1:8080`,如果我们希望本地代码的服务能够正常访问后端，或者访问到测试机的真实数据，这就跨域了，因为本地和后端或者测试的都不在一个域名下，是无法访问到数据的，通常的做法是启动一个Nginx服务来进行转发，这一个过程很麻烦，配置改来改去，而且如果要做到，代码修改实时看到效果，那就更复杂了。。。

**解决方案： **
我的做法是： 将本地服务`127.0.0.1`代理到某个域名比如`www.cloud.com`,这时候访问这个域名就能看到自己的项目了，这个域名下也是木有后端或者测试机的接口的，怎么办了，继续代理转发，将域名下所有的接口代理到真实的IP下，比如后端或者测试机接口的服务在 `10.50.12.24:8088`下，那我们就将`www.cloud.com/order`[order是后端接口固定的项目名，每个接口都在这个目录下]指向`10.50.12.24:8088`下，完美解决；

![](https://user-gold-cdn.xitu.io/2017/9/19/c88065ebf2a4b8040301812671025009)
顺着这个思路3步解决，
1. 在willow中添加项目`快捷键ctrl+p`；
2. 添加host代理`ctrl + o`;
3. 添加指向add exten `ctrl + n`;
> 不懂的童鞋看图

图一：![](https://user-gold-cdn.xitu.io/2017/9/19/e888cb3b5523fc8e4b2ae5e75965a63b)
图二：![](https://user-gold-cdn.xitu.io/2017/9/19/c9aa5ff11ffcda12670d8e35ed0d9e4d)
好了可以开心的联调代码了；
ps： 这个checkbox框框表示是否启用，如果想去掉，点掉就好了；比手动去修改host方便多了；
#### To 后端
后端大侠有时候想这么干，用测试机的前端代码，来访问自己本地的服务，这样就不用浪费前端童鞋的资源了，测试机上自己测试；
这个步奏其实就是上面的逆过程，简单多了，测试机的前端代码自然指向测试机的后端代码，使用上面思路的第三步；添加指向，指向本机，这里要注意两个点：
1. 直接代理接口`www.cloud.com/order` --> 本地服务+端口
2. 如果是https的，需要后端给tomcat配置https证书

介绍就先到这里了，更多使用技巧，后续在更新。。。

### 后记
后端某个功能bug，不断地让前端点击按钮发请求，他来打断点，有时候我比较懒，就让后端自己点，后端如何访问前端开发机电脑上的代码，来访问自己电脑上的后端服务？
这个问题大家可以思考下，下期分享。。。
