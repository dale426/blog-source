---
title: css换行与不换行
tags: [CSS]
categories: [CSS]
date: 2018-12-20 19:36:25
---
摘要：css实现换行与不换行
<!-- more -->
#### **white-space**

> 其值：*normal|pre|nowrap|pre-wrap|pre-line|inherit*;  属性设置如何处理元素内的空白

 - `normal` 默认,空白会被浏览器忽略。
 - `pre`    空白会被浏览器保留。其行为方式类似 HTML 中的 pre 标签。
 - `nowrap`   文本不会换行，文本会在在同一行上继续，直到遇到 br 标签为止。
 - `pre-wrap` 保留空白符序列，但是正常地进行换行。
 - `pre-line` 合并空白符序列，但是保留换行符。
 - `inherit`  规定应该从父元素继承 white-space 属性的值。


#### **word-wrap**
> *normal|break-word*; 用来标明是否允许浏览器在单词内进行断句，这是为了防止当一个字符串太长而找不到它的自然断句点时产生溢出现象。

- `normal`: 只在允许的断字点换行(浏览器保持默认处理)
- `break-word`:在长单词或URL地址内部进行换行


#### **word-break**
> *normal|break-all|keep-all*; 用来标明怎么样进行单词内的断句。

- `normal`：使用浏览器默认的换行规则。
- `break-all`:允许再单词内换行
- `keep-all`:只能在半角空格或连字符处换行


#### 省略号显示超出文本

```css
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    width: 160px;
```
> [预览地址](http://93ddcf18.wiz03.com/share/s/2jTsYo1emAvX20o-rU2PV3VR0ssCns0LlQ4e25nDQ70e2ldW)