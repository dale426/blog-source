title: 一道简单的算法题目， 我居然用了一个小时 ~~
tags: ['算法', 'js 知识点总结']
categories: ['前端开发']
date: 2019-06-06 13:14:00

> 想想闭包~

<!-- more -->

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
    var temp = arguments[0];
    if (temp) {
      num *= temp; // 计算当前的值
      return sub; // 递归调用
    } else {
      // 没有参数了 需要返回前面存储的值
      // 应为这是个闭包函数， num的值是不会被销毁的
      let tempNum = num
      num = 1 // 重置， 否则下次调用 外层的 fn 函数 会将之前的结果集 累乘起来
      return tempNum;
    }
  };
})();
```

> 总结: 写代码前 先构思伪代码， 不能一上来就开始写， 容易进入误区 ~
