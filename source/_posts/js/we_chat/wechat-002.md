---
title: 微信小程序跳转总结
tags: [小程序]
categories: [前端开发]
date: 2018-12-11 16:19:36
---
简介: 总结微信小程序开发过程中如何正确使用跳转
<!-- more -->
#### 小程序跳转问题
[官方地址传送门](https://developers.weixin.qq.com/miniprogram/dev/api/wx.navigateBack.html)
##### `wx.navigateBack` 

> 小程序返回上一页或者多级页面。可以使用 `getCurrentPages()` 获取当前的页面栈，决定需要返回几层。

```js
wx.navigateBack({
  delta: 2 // 返回上一页 层级
})
```

##### `wx.navigateTo()` 可以传参
***
> wx.navigateTo(OBJECT) 保留当前页面，跳转到应用内的某个页面，使用wx.navigateBack可以返回到原页面
**跳转的页面会自带返回按钮**

```js
wx.navigateTo({
  url: 'test?id=1'
})

// test.js
Page({
  onLoad(option) {
    console.log(option.query)
  }
})
```

##### `wx.reLaunch()`可以传递参数

> 关闭所有页面，打开到应用内的某个页面
**跳转的页面不会有返回按钮**

##### `wx.switchTab()`不能带参数

> 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
**跳转的页面不会有返回按钮**

##### `wx.redirectTo()` 可以传递参数

> 关闭当前页面，跳转到应用内的某个页面。

```js
    wx.redirectTo({
        url: 'test?id=1'
    })
```
***
**案例问题？**
> 页面A-【navigateTo】 -> login页面-【navigateTo】 -> 页面C（该页面需要登录，所以在A跳转的时候判断是否登录，如未登录跳转到登录页面）
> 开始时跳转都用的是 navigateTo, 在页面C中点击返回时，返回到了登录页，这显然是错误的，一直在想如何从C页面直接跳回A页面....

解决方法： 在login页面使用 `wx.redirectTo`

预期效果：A -> login -> C    然后  C直接返回A

A -> login  通过 `wx.navigateTo` 跳转

login -> C 通过 `wx.redirectTo` 跳转.跳转触发后 login 页面就会被销毁， C 页面再返回 `wx.navigateBack` 就会直接到 A 了