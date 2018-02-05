---
title: hexo 命令集合
date: 2017-10-31 13:55:00
description: "hexo 操作的一些命令"
---
<Contents>
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
$ hexo s
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### [发布]Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

http://blog.csdn.net/qq_23435721/article/details/50997275

### 添加搜索的方法

``` bash
npm install hexo-generator-search --save
```

More info: [站内搜索](http://moxfive.coding.me/yelee/2.Basic-Usage/local-site-search.html)
More info: [github地址](https://github.com/PaicHyperionDev/hexo-generator-search)

### seo优化

``` bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

More info: [优化教程](http://blog.csdn.net/qq_23435721/article/details/50997275)
More info: [优化教程](http://www.jianshu.com/p/86557c34b671)
More info: [优化教程](http://www.arao.me/2015/hexo-next-theme-optimize-seo/)


### 首页显示tag的两种方法

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