---
title: 微信小程序开发采坑指南
tags: [小程序]
categories: [前端开发]
date: 2018-09-20 17:33:18
---
tips: 总结微信小程序开发过程中遇到的一些坑
<!-- more -->
####  block标签
`<block/>` 并不是一个组件，它仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性。
> 例如 wx:if  wx:for

```html
<block wx:if="{{true}}">
  <view> view1 </view>
  <view> view2 </view>
</block>
```
#### 小程序中的背景图片
##### 实现背景图的方法
> tips： `background-image` 只能用网络url或者base64 . 本地图片要用`image`标签才行。

可以通过image标签和其他元素层叠来实现背景图，使用 `position：absolute`定位层叠；

小技巧：

实现如图效果：
![图片](/public_s/images/menu.saveimg.savepath20180920175019.jpg)
采用元素溢出，和`position:relative`定位来实现
> html
```html
<view class='wrap-center-line'>
    <view class='line-bac'>
        <text class='line-bac-text'>车次详情</text>
    </view>
    </view>
```
> css
```css
.wrap-center-line {
  height: 40rpx;
  padding: 12rpx 32rpx;
  display: flex;
  align-items: center;
}
.wrap-center-line .line-bac {
  width: 100%;
  height: 2rpx;
  border-top: 2rpx dashed #d9d9d9;
  text-align: center;
}
.wrap-center-line .line-bac-text {
  display: inline-block;
  position: relative;
  top: -24rpx;
  background: #fff;
  color:#A6A6A6;
  padding: 0 24rpx;
}
```
##### 动态图片显示
> 动态改变图片，可以在url中使用双括号的方式来绑定数据

`<image class="swiper-img" mode='aspectFill' src='../../assets/swiper/{{item}}'></image>`

#### 触发事件传值

在view层绑定事件，将当前视图对应的数据传递到事件中去，方法如下
> html
```html
    <view bindtab="{{ clickHandle }}" data-batchNumber='{{batchNumber}}'> 点击按钮 </view>
```
在js事件中接收时` data-batchNumber` N会默认转换成小写字母

```js
clickHandle: function(event) {
    //假定batchNumber的值是 P132456789
    console.log(event.currentTarget.dataset)
    // 输出的是 { batchnumber: 'P123456789' } 注意到： 其中的 n 是小写的
    // 应该如下方法获取
    let { batchnumber } = event.currentTarget.dataset
}
```

#### 小程序使用iconfont

小程序中使用iconfont其实和PC上一样，只需要将从iconfont上下载下来的 iconfont.css 文件的后缀修改成 iconfont.wxss
-  将iconfont.wxss文件放在小程序根目录下，在 app.wxss 中引入
    `@import "icon-font.wxss";`
-  页面使用：
    `<text class='iconfont icon-ic_dianhua'></text>`

