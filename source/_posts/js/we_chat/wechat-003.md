---
title: 微信小程序开发采坑指南(二)
tags: [小程序]
categories: [前端开发]
date: 2018-12-19 15:10:35
---
> 内容：小程序转发
<!-- more -->
### 小程序转发

微信小程序当页面js中有onShareAppMessage函数，右上角就会有转发按钮， 默认转发的当前页面；转发时的图片就是当前页面80%的高度；
[官方API](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share.html)
[onShareAppMessage](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html#%E9%A1%B5%E9%9D%A2%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)
```js
/**
* 用户点击右上角分享
*/
 onShareAppMessage: function (ops) {
   if (ops.from === 'button') {
        // button：页面内转发按钮；
        // menu：右上角转发菜单
     console.log(ops.target)
   }
   return {
     title: '我的小程序',
     path: 'pages/index/index', //转发链接打开的页面
     imageUrl: 'img/自定义图片路径', //没有时使用当前页面内容
       // 转发成功
     success: function (res) {
       console.log("转发成功:" , res);
     },
       // 转发失败
     fail: function (res) {
       console.log("转发失败:" ,res);
     }
   }

 }
```
> 转发成功后，小程序会自动显示一个 tips  转发成功 ；