---
title: vue中的计算属性 computed
date: 2017-11-07 19:49:48
description: vue中使用 computed 时，改变对象的属性，vue无法检测到，通常直接改变这个对象的索引；
tags: [vue]
categories: [vue框架]
---
## vue中使用计算属性 computed
> html Vue:

```vue
<template>
    <div>
        <p>{{cpu_number}}</p>
        <p>{{cpu_numObj}}</p>

        <p>{{cpu_numObj2}}</p>
    </div>
</template>
```
> javaScript:

```js

data() {
    return {
        number: 1,
        numObj: {},
        numObj2: {},
    }
},
computed: {
    cpu_number: function() {
        return this.number ++
    },
    cpu_numObj: function() {
        this.numObj.type = '直接改变对象属性'    // 计算属性不会检测到
        return numObj.type
    }
    cpu_numObj2: function() {
        this.numObj2 = Object.assign({}, {type: '改变了对象索引'})    // 计算属性会检测到
        return numObj2
    }
}
```

**总结**： vue 计算属性computed可以检测到 `变量`  `数组`  `对象的`变化； 但是 对象的属性变化是不会被检测到的