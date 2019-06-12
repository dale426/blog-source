---
title: Vue父子组件通信与生命周期执行顺序
tags: [vue]
categories: [vue框架]
date: 2019-03-13 14:32:00
---

Vue父子组件嵌套如何传值，以及生命周期的执行顺序是如何的~
<!-- more -->

*题外话： 最近变得懒惰了， 工作也不是特别忙，学习充电也没跟上，最近项目开发遇到了一些问题，记录总结下；*

vue框架有个优点就是，可以将`.vue`文件直接用作组件去使用，

#### **v-bind** 属性
```html
 使    用：     <blog-post v-bind:likes="42"></blog-post>
 简写方式：     <blog-post :title="Mytitle"></blog-post>
 传入一个对象：  <blog-post v-bind="myObject"></blog-post>  // myObject = {}
```
##### 在子组件中加工或者修改props
1. props只是当做初始值， 在子组件中维护自己的状态，需要在组件的data中定义属性，并将props的值赋予即可；
```js
props: ['title'],
data() {
    return {
        childTitle: this.title
    }
}
```
2. props值在使用前需要进行转换，这种情况一般定义一个计算属性；
```js
props: ['dataList'],
computed() {
    fileList() {
        return dataList.map( (item) => {
            return item.size * 10
        } )
    }
}
```
**注意：** props传递数组、对象这种引用类型的数据时，改变其值将会影响父组件中的数据状态；
****
#### **v-on** 事件

##### 事件名使用格式
> 事件名不会被用作一个 JavaScript 变量名或属性名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent——导致 myEvent 不可能被监听到。
    因此，事件名使用kebab-case格式，使用示例： `v-on="my-event"` 或者 `@on-change="Event"`。
##### 父组件监听子组件事件  
> 即  子组件**主动**将数据回传父组件 或者 改变父组件的状态

1. 父组件中绑定事件
```html
<!-- html -->
<template> 
    <div>
        <my-components @on-confirm="getData"> </my-components>
    </div>
</template>

<!-- javascript -->
methods: {
    getData(data) {
        // do something ...
    }
}
```

2. 子组件使用自定义事件系统 $emit 触发事件
```js
this.$emit('on-confirm', myData); //触发事件，并将myData数据传递给父组件
```

##### 父组件访问子组件方法、属性

> vue提供了访问组件实例的方法，可以通过实例读取子组件的实例上的方法和属性；

父组件访问子组件属性或者方法
```js
this.$refs['组件绑定的ref'].title      // 访问子组件data中的属性
this.$refs['组件绑定的ref'].queryList() // 访问子组件中的方法
```
调用子组件的方法时，我们可以巧妙的传递一个回调函数， 在子组件中将数据传回的父组件；
```js
 // 父组件
    this.$refs['组件绑定的ref'].queryList( data => {
        // do something...
    }) 
// 子组件
queryList(callback) {
    let mydata = [1, 2, 3];
    if(typeof callback === 'object') {
        callback(mydata); // 回传数据
    }
}
```
#### v-if条件渲染

> v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

如果给DOM元素 或者 组件使用了v-if， 他们都会根据条件渲染，组件执行生命周期函数渲染；
v-if与v-show的区别是： 
- v-if： 组件会根据v-if进行重建和销毁； 【有更高的切换开销】
- v-show：第一次就会加载渲染DOM，值变化时，只是对CSS进行了变化；【有更高的初始渲染开销】

如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

#### 父子组件生命周期执行顺序

首先 vue 中生命周期函数顺序是 `**created** -> **mounted** -> **destroyed**`

抛出问题： 页面是有若干组件组成， 组件中可能会再用组件， 业务需求是父组件获取数据后， 传递给子组件， 子组件在接到数据后，渲染在页面， 或者通过接收到的数据在进行异步请求获取数据等，那么子组件在什么阶段才能获取到父组件传递过来的props值呢？

先来看下 父子组件的生命周期执行顺序>
![图片](/public_s/images/3793524098-5b665dbbde824_articlex.png)


一般情况下父组件都是在mounted时机进行异步请求,然后才能获取到数据，显然，当父组件mounted执行时， 子组件的生命周期都已经执行完了；如果子组件需要对数据进行加工、筛选的方法在什么时机去执行呢 ？

**解决方案：**

**方案一、**根据什么周期执行顺序， 如果我们可以在父组件的created阶段获取到数据【已经获取到】，这样在子组件的生命周期中调用方法，可以正确拿到props数据
**方案二、**给子组件添加 v-if="false"，等父组件在获取到数据后，设置 v-if 为 true，这时候子组件才开始进行渲染加载， 生命周期执行是可以获取props数据，
        但是这样做的缺点是，异步数据获到之前，页面是空白的，因为子组件添加了 v-if="false", 不会渲染；
**方案三、**子组件中使用watch来监听props的变化然后调用方法，当异步获取到数据后，watch会监听到props更新；目前该方法是比满足需求的；
```js
watch: {
    dataList: {
        handler(newVal) {
            this.changeData();
        },
        immediate: true // 若使用v-if，加载子组件，watch第一次是不会被触发的，所以开启immediate为 true
    }
}
```