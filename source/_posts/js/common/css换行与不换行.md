---
title: css换行与不换行
tags: [CSS]
categories: [CSS]
date: 2018-12-20 19:36:25
---

摘要：css 实现换行与不换行

<!-- more -->

#### **white-space**

> 其值：_normal|pre|nowrap|pre-wrap|pre-line|inherit_; 属性设置如何处理元素内的空白

- `normal` 默认,空白会被浏览器忽略。
- `pre` 空白会被浏览器保留。其行为方式类似 HTML 中的 pre 标签。
- `nowrap` 文本不会换行，文本会在在同一行上继续，直到遇到 br 标签为止。
- `pre-wrap` 保留空白符序列，但是正常地进行换行。
- `pre-line` 合并空白符序列，但是保留换行符。
- `inherit` 规定应该从父元素继承 white-space 属性的值。

#### **word-wrap**

> _normal|break-word_; 用来标明是否允许浏览器在单词内进行断句，这是为了防止当一个字符串太长而找不到它的自然断句点时产生溢出现象。

- `normal`: 只在允许的断字点换行(浏览器保持默认处理)
- `break-word`:在长单词或 URL 地址内部进行换行

#### **word-break**

> _normal|break-all|keep-all_; 用来标明怎么样进行单词内的断句。

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
---
<style>
    .main_code {
        width: 400px;
        border: 2px solid lightgreen;
        display: flex;
        flex-direction: column;
    }
    .main_code .wrap-line {
        display: flex;
        margin-bottom: 5px;
    }
    .main_code .title {
        width: 150px;
        flex-shrink: 0;
        text-align: right;
        /* color: red; */
    }
    .main2 {
        width: 400px;
        border: 2px solid lightsalmon;
        display: flex;
        flex-direction: column;
    }
    .main2 .wrap-line {
        display: flex;
        margin-bottom: 5px;
    }
    .main2 .title {
        width: 150px;
        flex-shrink: 0;
        text-align: right;
        /* color: red; */
    }
    .main2 .a-content,
    .main2 .d-content,
    .main2 .b-content {
        white-space: nowrap;
    }
    .main2 .aa-content {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
    .main3 {
        margin-top: 5px;
        width: 400px;
        border: 3px solid rgb(235, 24, 154);
        display: flex;
        flex-direction: column;
    }

    .main3 .wrap-line {
        display: flex;
        margin-bottom: 5px;
    }
    .main3 .title {
        width: 150px;
        flex-shrink: 0;
        text-align: right;
        /* color: red; */
    }
    .main3 .c-content,
    .main3 .e-content {
        word-break: break-all;
    }
    .last-content li{
        line-height: 2em;
    }
</style>

<div>
    **默认情况下**：
    我们发现连续数字和连续字母不进行换行，而是会撑破盒子；
	<div class="main_code">
		<div class="wrap-line">
			<label class="title" for="">默认情况【中文】：</label>
			<div class="a-content"> 我是一段很长的 文字我是一段很长 的文字我是一段很长的文字</div>
		</div>
		<div class="wrap-line">
			<label class="title" for="">有空格的英文：</label>
			<div class="b-content"> When did I say we could run through the rain and not get wet?</div>
		</div>
		<div class="wrap-line">
			<label class="title" for="">连续英文：</label>
			<div class="b-content" style="color:red;"> WhendidIsaywecouldrunthroughtherainandnotgetwet?</div>
		</div>
		<div class="wrap-line">
			<label class="title" for="">有空格的数字：</label>
			<div class="c-content">123456789456123 4567 8945612 345678 94123</div>
		</div>
		<div class="wrap-line">
			<label class="title" for="">连续数子：</label>
			<div class="c-content" style="color:red;">1234567894561234567894561234567894123</div>
		</div>
	</div>
	**强制不换行方法：**
    ```css
        div {
            white-space:nowrap;
        }
        div {
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
    ```
	<div class="main2">
		<div class="wrap-line">
			<label class="title" style="color:blue;" for="">中文：</label>
			<div class="a-content">我是一段很长的 文字我是一段很长的文字我是一段很长的文字</div>
		</div>
		<div class="wrap-line" style="color:green;">
			<label class="title" for="">中文：</label>
			<div class="aa-content">我是一段很长的 文字我是一段很长的文字我是一段很长的文字</div>
		</div>
		<div class="wrap-line">
			<label class="title" style="color:blue;" for="">有空格的英文：</label>
			<div class="b-content"> When did I say we could run through the rain and not get wet?</div>
		</div>
		<div class="wrap-line">
			<label class="title" style="color:blue;" for="">有空格的数字：</label>
			<div class="d-content">123456789456123 4567 8945612 345678 94123</div>
		</div>
	</div>
	**强制换行方法：**
```css
div {
    word-break: break-all;
}
```
	*注意：设置强制将英文单词断行，需要将行内元素设置为块级元素。
	<div class="main3">
		<div class="wrap-line">
			<label class="title" for="">连续英文：</label>
			<div class="c-content"> WhendidIsaywecouldrunthroughtherainandnotgetwet?</div>
		</div>
		<div class="wrap-line">
			<label class="title" for="">连续数子：</label>
			<div class="e-content">1234567894561234567894561234567894123</div>
		</div>
	</div>
</div>