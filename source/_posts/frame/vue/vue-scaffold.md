---
title: vue双向绑定原理学习（2）
tags: [vue]
categories: [vue框架]
date: 2019-04-10 19:32:32
---
摘要: 手写vue双向绑定实现原理
<!-- more -->
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>vue原理</title>
</head>
<body>
	<div id="vue_root">
		<h3 v-text="mybox"></h3>
		<p v-text="mytext"></p>
		<input type="text" v-model="mytext">
	</div>
</body>
<script>
	/* 数据响应式原理实现
	
	1. 首先构建 Vue类、 Watcher类， 使用订阅发布者模式
	2. 实现MVVM中的由 M 到 V， 把模型中的数据绑定到视图上
	3. 实现由 V 到 M， 当文本框输入时或者事件触发时 
		【1】 更新模型中的数据，
		【2】同时也更新相对应的视图 
		
	原理：

	vue实现数据响应式的原理利用了 Object.defineProperty()这个方法重新定义了对象获取属性值（get）和设置属性值（set）的操作来实现的
	vue 3.0 是通过ES6中的 Proxy对象来实现

	*/

	// 1. 创建 Vue 类
	class Vue {
		constructor(options) {
			this.$options = options
			this.$data = options.data
			this.$el = document.querySelector(options.el)
			this._directive = {} // 订阅者容器

			this.Observer(this.$data) // 劫持数据
			this.Compile(this.$el)    // 模板解析
		}
		// 劫持数据
		Observer(data) {
			for (let key in data) {
				this._directive[key] = [] // 初始订阅者容器  存放watcher 实例
				let val = data[key]
				let watch = this._directive[key]

				// this.$data中的每个属性 发生赋值时，都要更新视图
				Object.defineProperty(this.$data, key, {
					get: function () {
						return val
					},
					set: function (newVal) {
						// 当改变值时 更新视图
						if (newVal !== val) {
							val = newVal
							watch.forEach(element => {
								element.update()
							});
						}
					}
				})
			}
		}
		// 模板解析
		Compile(el) {
			let nodes = el.children // 获取 子元素节点
			for (let i = 0; i < nodes.length; i++) {
				let node = nodes[i]
				// 如果有子元素递归调用
				if (node.children.length) {
					this.Compile(node)
				}

				if (node.hasAttribute('v-text')) {  // hasAttribute 元素拥有指定属性
					let attrVal = node.getAttribute('v-text'); // 获取属性名
					this._directive[attrVal].push(new Watcher(node, this, attrVal, 'innerHTML'))  // 添加订阅者
				}

				if (node.hasAttribute('v-model')) {
					let attrVal = node.getAttribute('v-model');  // mytext
					this._directive[attrVal].push(new Watcher(node, this, attrVal, 'value'))  // 添加订阅者
					// 绑定事件、
					node.addEventListener('input', (function () {
						return function (event) {
							this.$data[attrVal] = node.value // event.target.value
						}
					})().bind(this)) // 使用闭包的方法， 直接使用function也是可以的
				}
			}
		}
	}
	// 2. 监听添加 订阅者
	// 【1】. 初始化视图
	// 【2】. 更新视图
	class Watcher {
		constructor(el, vm, exp, attr) {
			this.el = el
			this.vm = vm
			this.exp = exp
			this.attr = attr

			this.update()
		}
		// 更新视图
		update() {
			this.el[this.attr] = this.vm.$data[this.exp]
		}
	}
	// 创建vue实例
	new Vue({
		el: '#vue_root',
		data: {
			mytext: 'vue双向绑定原理实现',
			mybox: '这是一个box'
		}
	})
</script>
</html>
```