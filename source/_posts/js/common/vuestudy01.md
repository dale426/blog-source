---
title: vue原理学习笔记（一）
tags: ['vue原理']
categories: ['vue框架']
date: 2018-07-06 14:31:59
---
Vue响应式原理-将date数据变成可观察的（observable）
<!-- more -->

> 学习笔记-

第一步、将date数据变成可观察的（observable）;

怎么实现了，主要是利用的对象的 defineProperty属性！

```javascript
// 遍历对象，将对象的每个属性变成可观察的
function observe(data, callback) {
    Object.keys(data).forEach(function(key) {
        defineReactive(data, key, data[key], callback)
    })
}

function defineReactive(obj, key, data[key], callback) {
    Object.defineProperty(obj, key, {
        enumerable: true, //属性的可枚举性，for in是否能够遍历到
        configurable: true, // 表示能否通过delete删除属性从而重新定义属性
        get: function() {
            // 第一次执行`render`的时候进行收集，详见下一章
        },
        set: function(newVal) {
            callback() //回调执行render刷新视图
        }
    })
}


calss Vue {
    // 创建vue实例时将options中data指向了vue的实例， 即在VM实例上挂在了一个 `_data`属性;
    // 初始化调用了observe
    constructor(options) {
        this._data = options.data; // this指向实例
        observe(this._data, options.render)
    }
}

let app = new Vue({
    el: '#app',
    data: {
        text1: '1',
        text2: '2',
    },
    render() {
        console.log('render')
    }
})

```


上面的方法就简单的实现了将vue里面的data数据变成可观察的模式；如果改变了data里面属性的值就会触发对象的set， 从而触发订阅者的回调函数（如刷新视图）

