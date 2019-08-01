---
title: 一些简单的算法题
tags: ["算法"]
categories: ["JavaScript"]
date: 2019-06-06 13:14:00
---

算法对一个程序员应该也很重要的， 毕竟需要逻辑思维的整合；

<!-- more -->

### 第一题

题目是这样的：
编写一个 function 实现一下功能

- fn(1)(2)(3)() // 6
- fn(1)(2)(3)...(n) // 计算乘积 .

拿到这个题的时候第一眼看到 n， 应该是要 自调用 即 【递归】。
1. 于是 直接手撸开始...
```js
fn() {
  const temp = [].shift.call(arguments)
  if (temp) {
    fn()
  } else {
    ......
  }
}
```
  堵住了 不知道怎么弄了....
2. 尝试 2 ：
   值要保存， 应该要用到闭包
```js
const fn = (function() {
  let num = 1; // 用来保存值
  return function sub() {
    let temp = arguments[0];
    if (temp) {
      num *= temp; // 计算当前的值
      return sub; // 递归调用
    } else {
      // 没有参数了 需要返回前面存储的值
      // 应为这是个闭包函数， num的值是不会被销毁的
      let tempNum = num;
      num = 1; // 重置， 否则下次调用 外层的 fn 函数 会将之前的结果集 累乘起来
      return tempNum;
    }
  };
})();
```
3. 优化使用函数柯里化
```js
var currying = function (fn) {
  var args = []
  return function() {
    if (arguments.length) {
      [].push.apply(args, arguments);
      return arguments.callee
    } else {
      var arg2 = [...args];
      args=length = 0; // 成功计算一次后 清除闭包中保存的数
      return fn.apply(this, arg2)   
    }
  }
}

// 求乘积
var mul = (function () {
  return function() {
    return [].reduce.call(arguments, (total, next, index) => {
      return total * next
    }, 1)
  }
})();

var fn = currying(mul);

fn(5)(2)()   // 10
```
### 第二题

查找字符串 b 是否在字符串 a 中， 前提， 不能用 slice splice subString 正则；

无非就是一个一个的寻找嘛 for 循环解决

```js
function findStr(a, b) {
  let aArr = a.split("");
  let bArr = b.split("");
  for (let key in aArr) {
    if (aArr[key] == bArr[0]) {
      // 找到第一个起始位置
      let t = [];
      for (let i = 0; i < bArr.length; i++) {
        if (bArr[i] == aArr[Number(key) + i]) { // 注意 字符串 + 数字  = 字符串
          t.push(Number(key) + i);
        } else {
          break;
        }
      }
      // 如果找到就不用继续了
      if (t.length == bArr.length) {
        return key;
      }
    }
  }
  return -1;
}
```

** 总结: 写代码前 先构思伪代码， 不能一上来就开始写， 容易进入误区 ~**
