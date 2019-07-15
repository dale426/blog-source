title: hexo书写语法
date: 2017-10-31 00:00:00
tags: [博客]
categories: [学习]
description: "hexo的创建页面命令、内置的标签"
---
<Contents>

### hexo 创建一个新页面的方法

命令  `hexo new layout title tags`
> layout会默认使用scaffolds中的post模板来生成一个新页面

简写命令 `hexo new 'title' tags`

### 摘要内置标签

```js
  {% note class_name %} 内容 {% endnote %}
```
其中，class_name 可以是以下列表中的一个值：
- default
- primary
- success
- info
- warning
- danger

{% note %} default {% endnote %}
{% note primary %} primary... {% endnote %}
{% note success %} success... {% endnote %}
{% note info %} info... {% endnote %}
{% note warning %} warning... {% endnote %}
{% note danger %} danger... {% endnote %}


