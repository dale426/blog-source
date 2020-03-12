---
title: less不只是用来嵌套
tags: [Less]
categories: [CSS]
date: 2020-03-12 10:25:18
---
tips: less不只是用来写嵌套的，还有很多实用功能和技巧
<!-- more -->



<a name="KMdLg"></a>
## 开始之前
> 介绍下如何实时调试学习less；

1. 安装less依赖

```shell
npm install -g less
```

2. 编写.less文件, 然后进行编译

```shell
 npx lessc xxx.less
```

以上命令就可以在控台输出编译过后的css，如果想编译后在一个文件中显示，使用如下命令：

```shell
npx lessc xxx.less xxx.css
```

如此就会在目录中看到编译过后的css了；

---

## less使用指南
——正文开始——
<a name="r90JF"></a>
### 变量

```less
@width: 10px;
@height: @width + 10px;
```

<a name="VDI2w"></a>
### 混合
<a name="TxsHS"></a>
#### 基础混合
```less
.bese {
	color: red;
  font-size: 14px;
}

.header {
  .base();  /* 可以使用一个类当做函数混合使用 */
  background: #fff;
}
```

<a name="TRj93"></a>
#### 带参数的mixins
Height mixin 支持传入一个参数来确定高度和行高，默认为 10px

```less
:root {
  --primary: red;
}
.long(@w: 10px, @h: 1px, @pd) {
  width: @w;
  height: @h;
  color: @pd;
}
.main8 {
  .long(@w: 700px, 30px, var(--primary));
}
```

不定传参

```less
.padding(...){
    padding: @arguments;
}
.div1{
    .padding(1px, 2px)
}
.div2{
    .padding(1px, 2px, 3px)
}
```

<a name="31d14"></a>
### 嵌套

```less
.head {
  font-size: 14px;
  .title {
    color: red;
  }
  .nav {
  	width: 200px;
  }
}

/*  & 表示当前选择器 .clearfix */
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}

/*  & 拼接用法 .clearfix */
.main {
    margin: 10px;
    &_nav {
        border: 10px solid red;
    }
    &-aside {
        padding: 5px;
    }
}

/ ** 编译后： **/
.main {
  margin: 10px;
}
.main_nav {
  border: 10px solid red;
}
.main-aside {
  padding: 5px;
}

```

<a name="3iFcv"></a>
### 运算

- 算术运算符 `+`、`-`、`*`、`/` 可以对任何数字、颜色或变量进行运算
- 如果可能的话，算术运算符在加、减或比较之前会进行单位换算。
- 计算的结果以最左侧操作数的单位类型为准

```less
@base: 2cm * 3mm;  // 结果是 6cm
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%

```

<a name="K4Av5"></a>
#### calc特例

为了与 CSS 保持兼容，`calc()` 并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。

```less
@var: 50vh/2;
width: calc(50% + (@var - 20px));  // 结果是 calc(50% + (25vh - 20px))
```



<a name="aIc0T"></a>
### 转义
转义（Escaping）允许你使用任意字符串作为属性或变量值。任何 `~"anything"` 或 `~'anything'` 形式的内容都将按原样输出，除非 [interpolation](https://less.bootcss.com/features/#variables-feature-variable-interpolation)。

```less
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

 /* 3.5+版本中也可以这样写 */
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```
编译后：

```less
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

<a name="AsT5J"></a>
### 
<a name="NASC8"></a>
### 命名空间和访问符号

有时候需要将一些 css 方法封装在一个命名的空间中，不影响其他的样式可以使用, 可以使用 **#** 或者** .**

- _**#method() { .class {} }**_
- **.method() { .class {} }**

```less
#myclass() {  /* 定义可一个 myclass 方法，将常用的class方法放在其中 */
  .title {
  	color: red;
    padding: 12px;
  }
  .content {
    border: 2px solid red;
  }
  .classa {}
  .classb {}
}
 /* 使用 */
.main {
	font-size: 20px;
  #myclass.title();  /* 访问符号 或者 #myclass > .title; */
  #myclass > .content;
}
```

编译后：

```css
.main {
  font-size: 20px;
  color: red;
  padding: 12px;
  border: 2px solid red;
}
```

**注意：** 如果 `#myclass()`  不写 `()` 直接写成 `#myclass {...}` 将会输出到编译后的 css 中

去掉括号编译如下：
```css
/* 方法也输出在css中了--- */
#myclass {
}
#myclass .title {
  color: red;
  padding: 12px;
}
#myclass .content {
  border: 2px solid red;
}
/* 方法也输出在css中了--- */
.main {
  font-size: 20px;
  color: red;
  padding: 12px;
}
```

<a name="ItB8k"></a>
### 映射
<a name="yGdbc"></a>
#### 嵌套映射
普通样式中的嵌套都可以使用映射 [] 直接取值，自定义的属性也能直接取值， 命名空间中的嵌套也能直接取值；
```less
#myclass() {  /* 定义可一个 myclass 方法，将常用的class方法放在其中 */
  .title {
    color: red;
    padding: 12px;
  }
  .content {
    border: 2px solid red;
  }
  primary: blue; /* 自定义的属性 */
  secondary: green; /* 自定义的属性 */
}

 /* -------------------------------------------------------------- */

.youclass() {
  border: 1px solid blue;
  .nav {
    color: darkblue;
  }
  .tap {
    padding: 20px;
  }
  mycolor: red;
}

 /* -------------------------------------------------------------- */

 /* 使用 */
.main2 {
  font-weight: bold;
  color: #myclass[primary]; /* 使用映射 */
  background: #myclass[secondary]; /* 使用映射 */
}

.main3 {
  border: .youclass[border];
  color: .youclass[mycolor];
  .youclass.tap();
}
```

编译后：

```css
.main2 {
  font-weight: bold;
  color: blue;
  background: green;
}
.main3 {
  border: 1px solid blue;
  color: red;
  padding: 20px;
}
```

<a name="HabEB"></a>
#### 规则集映射
可以将变量规则集合作为映射；

```less
@color: blue;
 /* 规则集合 */
@colorList: {
  first: red;
  second: red - 100;
  third: @color - 30;
}
.app {
  color: @color;
  border-color: @colorList[second]; /* 使用规则集合 */
}
```

编译后

```css
.app {
  color: blue;
  border-color: #9b0000;
}
```


<a name="OFy7S"></a>
### 作用域
Less 中的作用域与 CSS 中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

与 CSS 自定义属性一样，混合（mixin）和变量的定义不必在引用之前事先定义。因此，下面的 Less 代码示例和上面的代码示例是相同的：

```less
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```


