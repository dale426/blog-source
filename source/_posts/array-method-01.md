---
title: 每个JavaScript程序员都需要知道的5个数组方法
date: 2017-11-7 15:16:00
description: "操作数组的5个方法，forEach、map、filter、indexOf、every"
tags: [数组]
categories: [JavaScript]
---
### Array.forEach()
 .forEach() 方法能够方便的让你 遍历数组里的每个元素，你可以在回调函数里对每个元素进行操作。
 .forEach()方法没有返回值，你不需要在回调函数里写return，这是无意义的。
```js
var animals = ['dog', 'cat', 'mouse'];

animals.forEach(function(item){
    console.log(item);
});
```
</br>
### Array.map()
 .map() 方法能够遍历整个数组，然后 返回一个新数组，这个新数组里的元素是经过了指定的回调函数处理过的。
如果你想修改数组里的每个元素，然后将修改后的数组存入新的数组，那使用 .map() 方法最方便。

```js
var numbers = [2, 4, 6, 8];

var doubleNums = numbers.map(function(element) {
    return element * 2;
});
console.log('doubleNums: ', doubleNums)
```
</br>

### Array.filter()
 .filter() 方法能够 过滤掉数组中的某些元素，你可以在回调函数里设定条件，不符合条件的元素都会排除在外。
```js
var scores = [3, 12, 5, 23, 19, 7];

var topScores = scores.filter(function(item){
    if (item > 10){
        return true;
    } else {
        return false;
    }
});

console.log('topScores: ', topScores);
```
</br>

### Array.indexOf()
 indexOf() 能够告诉你 某个元素在数组中的位置，它返回的是索引值，如果数组里有重复的元素，它会返回第一个元素的位置。
```js
var a = [2, 9, 9, 18];

var i = a.indexOf(9);

console.log('i: ', i);


/*
if (a.indexOf(7) === -1) {
  // 数组中没有这个元素
}*/
```
</br>

### Array.every()
 .every() 方法的作用是用指定的回调函数去检查数组中的每个元素，如果对于每个元素，这个回调函数都返回true，则.every()返回true。否则，.every() 返回false。
如果你想知道数组中的所有元素都是否符合某种条件，使用 .every() 最方便。
```js
var ages = [23, 19, 32, 44];

var olderThan18 = ages.every(function(element) {
    return element > 18;
});

console.log('olderThan18: ', olderThan18);
```

上面的这5个方法只是很多JavaScript方法中关于数组的最重要的几个，还有很多关于数组的方法、工具包(lodash and underscore)等都非常的有用。
