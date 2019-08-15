---
title: CSS变量
tags: ['css']
categories: ['CSS']
date: 2019-08-15 16:59:45
---
随着Web应用程序变得越来越大，CSS变得越来越大，越来越多，而且很多时候都很乱，在良好的上下文中使用CSS变量，为您提供重用和轻松更改重复出现的CSS属性的机制。 在“纯粹的”CSS支持变量之前，我们有像Less和Sass这样的预处理器。但是它们需要在使用前进行编译，因此（有时）会增加额外的复杂性。
<!-- more -->

### 变量的声明
在变量名前面添加两根连词线`--`
```css
body {
  --bg: #7f583f;
  --fz: 20px;
}
```
变量名对大小写敏感；

### var()函数

var()用于读取变量；
```css
.gtoup {
  font-size: var(--fz);
  background: var(--bg);
}
```
`var()`函数还可以使用第二个参数，表示变量的默认值，如果变量不存在就是用默认值。
```css
.group2 {
  color: var(--co, green);
}
```
第二个参数不处理内部的逗号或者空格，都会视为一个参数；
```css
var(--font-stack, "Ro boto", "Helvetica");
var(--pad, 10px 15px 20px);
```

`var()`函数中还可以使用变量声明
```css
:root {
  --primary-color: red;
  --logo-text: var(--primary-color);
}
```
### 变量值的类型

1. 如果变量值是一个字符串，可以与其他字符串拼接；
```css
:root {
  --bar: 'hello';
  --foo: var(--bar)' 你好';
}
```
> 实例
```css
body::after {
  content: var(--foo)'12345';
  /* 输出 hello 你好 12345 */
}
```

2. 数字不可以直接连接，需要使用`calc()`函数链接

```css
:root {
  --mt: 20;
}
.group {
  margin-top: calc(var(--mt) * 1px);
}
```
3. 如果变量值带有单位，就不能写成字符串。
```css
/* 无效 */
.foo {
  --foo: '20px';
  font-size: var(--foo);
}

/* 有效 */
.foo {
  --foo: 20px;
  font-size: var(--foo);
}
```
### 作用域

变量的作用域读取时，优先级高的生效，和css层叠一致；
```css
body {
  --foo: green;
}

.content {
  --foo: red;
}
```
在上面的例子中，--foo在选择器为 `.content`的范围内优先级更高；

因此通常将全局的变量放在根元素`:root`中；
> `:root`选择器用匹配文档的根元素。    
在HTML中根元素始终是HTML元素。

### 响应式布局

可以在响应式布局的media命令中声明变量，使得不同的屏幕宽度有不通的变量值；

```css
/* 普通读取这个 */
body {
  --bg: red;
  --secondary: #F7EFD2;
}
/* 宽度大于768时 读取这个 */
@media screen and (min-width: 768px) {
  body {
    --bg:  green;
  }
}

.group2 {
    background: var(--bg);
}
```

### 兼容性处理

对于不支持css变量的属性，使用如下写法；
```css
.group {
    color: red;
	color: var(--bg);
}
```

或者使用`@support`检测；
```css
@supports ( (--bg: 0)) {
  /* supported */
}

@supports ( not (--bg: 0)) {
  /* not supported */
}
```

### JavaScript操作CSS变量

1. js检测浏览器是否支持CSS变量；
```js
const isSupported =
  window.CSS &&
  window.CSS.supports &&
  window.CSS.supports('bg', 0);

if (isSupported) {
  /* supported */
} else {
  /* not supported */
}
```
2. js操作CSS变量

```js
// 设置变量
document.body.style.setProperty('--bg', 'red');

// 读取变量
document.body.style.getPropertyValue('--bg').trim();
// 'red'

// 删除变量
document.body.style.removeProperty('--bg');
```