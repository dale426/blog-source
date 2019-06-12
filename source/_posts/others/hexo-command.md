---
title: hexo 命令集合
date: 2017-10-31 13:55:00
description: "hexo 操作的一些命令"
---


### 快速开始

#### 创建一篇文章

``` bash
$ hexo new "My New Post" "tags"
```

更多信息: [Writing](https://hexo.io/docs/writing.html)

#### 启动服务

``` bash
$ hexo server
$ hexo s
```

更多信息: [Server](https://hexo.io/docs/server.html)

#### 打包构建静态文件

``` bash
$ hexo generate
```

更多信息: [Generating](https://hexo.io/docs/generating.html)

#### [发布站点]

``` bash
$ hexo deploy
```

更多信息: [Deployment](https://hexo.io/docs/deployment.html)

http://blog.csdn.net/qq_23435721/article/details/50997275

#### 添加搜索的方法

``` bash
npm install hexo-generator-search --save
```

更多信息: [站内搜索](http://moxfive.coding.me/yelee/2.Basic-Usage/local-site-search.html)
更多信息: [github地址](https://github.com/PaicHyperionDev/hexo-generator-search)

#### seo优化

``` bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

更多信息: [优化教程](http://blog.csdn.net/qq_23435721/article/details/50997275)
更多信息: [优化教程](http://www.jianshu.com/p/86557c34b671)
更多信息: [优化教程](http://www.arao.me/2015/hexo-next-theme-optimize-seo/)


#### 首页显示tag的两种方法

1. 方法一
```
title: Hello World
date: 2000-12-03 00:00:00
---
<Excerpt in index | 首页摘要> 
+<!-- more -->
<The rest of contents | 余下全文>
```

2. 方法二
```
title: Hello World
date: 2000-12-03 00:00:00
+description: "Welcome to Hexo! This is your very first post."
---
<Contents>
```