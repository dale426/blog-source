---
title: 使用ES6中的Proxy实现双向绑定
tags: [ES6, vue]
categories: [vue框架]
date: 2019-04-12 15:53:52
---
摘要: 使用proxy代理实现双向绑定 
<!-- more -->
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
  <div>
    <p id="paragraph"></p>
    <input id="input" type="text">
  </div>
</body>
<script>
  /*
    只要更改proxyObj的属性就会触发代理事件、
      1. 更改model中的数据、
      2. 触发视图的更新
    */

  // 获取节点
  var paragraph = document.getElementById("paragraph")
  var input = document.getElementById("input")
  // data数据
  var data = {
    text: 'ES6的Proxy实现双向绑定'
  }

  var handler = {
    set: function (target, prop, value) {

      /*  @{arguments}
      1. target 用Proxy包装的目标对象  例如下文中的 data
      2. props  目标对象的属性
      3. value更改的值 
      */

      if (prop === 'text') {
        target[prop] = value;
        // 更新视图
        paragraph.innerHTML = value
        input.value = value
        return true
      } else {
        return false
      }
    },
    /* 
    get: function(target, prop) {
      return target[prop] || '属性不存在'
    } 
    */
  }
  var proxyObj = new Proxy(data, handler); // 代理data对象

  input.addEventListener('input', function (event) {
    proxyObj.text = event.target.value  // 更改代理对象的属性值
  }, false)

  // 初始化
  proxyObj.text = data.text;

  /*
    proxy的优势
  
    1. Proxy可以直接监听对象而非属性
    2. Proxy可以直接监听数组的变化， 而Object.defineProperty只有规定的8种方法修改才可以
    3. Proxy返回的是一个新对象,我们可以只操作新的对象达到目的,而Object.defineProperty只能遍历对象属性直接修改。
   */
</script>
</html>
```