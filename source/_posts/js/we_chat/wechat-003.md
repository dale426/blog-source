---
title: 微信小程序开发总结(二)
tags: [微信小程序]
categories: [小程序]
date: 2018-12-19 15:10:35
---
内容：小程序转发、小程序组件使用
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


### 小程序组件使用

父页面使用组件， 传递两个属性， propMydata 、myTitle


```html
<my-component prop-mydata="{{mydata}}" my-title="{{mytitle}}">
<!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
    <view>这里是插入到组件slot中的内容</view> 
</my-component>
```
```js
Page({
    data: {
        mydata: [1,2.3,4],
        mytitle: 'this is a props'
  },
})
```

组件页面接收props
```html
<view>
  <my-component prop-mydata="{{mydata}}" prop-title="{{mytitle}}">
    <!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
     <view>这里是插入到组件slot中的内容</view> 
  </my-component>
  <view>{{title}}</view>
</view>
```

```js
// 组件js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
    propMydata: { //props名字
      type: Array, //类型（必填），目前接受的类型包括：String, Number, Boolean, Object, Array, null（表示任意类型）
      // value: {}, //属性初始值（可选），如果未指定则会根据类型选择一个
      observer: function(newVal, oldVal, changedPath) {
        // 属性被改变时执行的函数（可选），也可以写成在methods段中定义的方法名字符串, 如：'_propertyChange'
        // 通常 newVal 就是新设置的数据， oldVal 是旧数据
      }
    },
    propTitle: String
  },
})
```