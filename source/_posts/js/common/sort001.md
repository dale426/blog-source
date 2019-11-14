---
title: 经典算法-选择排序&冒泡排序
tags: ["算法"]
categories: ["JavaScript"]
date: 2019-11-14 11:10:07
---
【重拾算法】: 入门经典算法回顾，冒泡与排序
<!-- more -->

### 选择排序法

*选择排序法*是在要排序的一组数中，选出最小（或最大）的一个数与第一个位置的数交换；在剩下的数当中找最小的与第二个位置的数交换，即顺序放在已排好序的数列的最后，如此循环，直到全部数据元素排完为止。
```js
var arr = [4, 1, 6, 8, 34, 23, 5, 6 ,89, 28, 33]
```
```js
function selectSort(sortArr) {
  let arr = JSON.parse(JSON.stringify(sortArr));
  let min;
  for (let i = 0, l = arr.length; i < l-1; i++) {
    min = i;
    // 查找最小的那个数的索引
    for (let k = i + 1; k < l; k ++) {
      if (arr[i] > arr[k]) {
        min = k;
      }
    }
    if (min !== i) {
      let temp = arr[i];
      arr[i] = arr[min];
      arr[min] = temp;
    }
    // 快速交换两个变量 [arr[i], arr[k]] = [arr[k], arr[i]] 
  }
  return arr;
}
```
![图片](/public_s/images/arithmetic/select.gif)

时间复杂度： 
- 最好的情况下，正序列排列，即min每次是最小的数，只执行了内层的循环 **n-1** 次，时间复杂度为 **O(n)**
- 最坏的情况下，内层基本语句的执行次数为**n+(n-1)+(n-2)+…+1 = n(n+1)/2**， 所以时间复杂度为O(n*n)

空间复杂度为: O(1)

### 冒泡排序法

冒泡排序算法的原理如下：

- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
- 对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
```js
function bubbleSort(sortArr) {
  let arr = JSON.parse(JSON.stringify(sortArr));
  for(let i = 0; i < arr.length - 1; i++) {  /* 外循环为排序趟数，len个数进行len-1趟 */
    for (let j = 0; j < arr.length - i -1; j ++ ) { /* 内循环为每趟比较的次数，第i趟比较len-i次 */
      if (arr[j] > arr[j+1]) {
        let temp = arr[j];
        arr[j] = arr[j+1];
        arr[j+1] = temp;
      }
    }
  }
  return arr;
}
```
![图片](/public_s/images/arithmetic/bubble.gif)

时间复杂度： 
- 最好情况下，正序排列，只需要内层循环 **n-1** 趟，所以时间复杂度为 **O(n)**
- 最坏情况下，需要进行 **n-1** 趟排序。每趟排序要进行 **n-i** 次关键字的比较, 所以内层的基本语句执行次数为 **(n-1)*n / 2**次， 时间复杂度为 **O(n\*n)**

空间复杂度：
**O(1)**