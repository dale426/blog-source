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