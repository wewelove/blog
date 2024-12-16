---
title: 使用 Hexo 搭建个人博客 - 体验 
date: 2016-04-07 21:20:35
categories: 
- 工具
tags: 
- hexo
- markdown
---

[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装

1. 安装 `Node.js` 环境， [官网](https://nodejs.org/en/) 

    [参考：Node.js 之旅 - 好的开端](/2015/03/15/nodejs-start/)

2. 安装 `Git` 版本管理工具，[官网](https://git-scm.com) 
    
    [参考：](nodejs-start.md)

3. 安装 `hexo-cli` 命令行工具，初始化一个项目

    ```sh
    $ npm install -g hexo-cli
    $ hexo init blog
    $ cd blog
    $ npm install
    ```

    目录结构大致如下：
    
    ```sh
    ├── _config.yml             # 项目配置文件
    ├── node_modules            
    ├── package.json
    ├── scaffolds               # 模板目录
    ├── source                  # 文章源目录
    │   └── _posts              # 文章目录
    │       └── hello-world.md  # 第一篇文章
    ├── themes                  # 主题目录
    │   └── landscape           # 默认主题
    │       ├── _config.yml     # 主题配置文件
    │       ├── Gruntfile.js
    │       ├── languages       # 主题语言设置
    ```

3. 启动本地服务，查看效果

    ```sh
    $ hexo server
    INFO  Start processing
    INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
    ```

    用浏览器打开命令行中输出的地址就可以查看效果了。

## 写文章

1. 新建一篇文章

    ```sh
    $ hexo new hexo-blog
    ```
    此时 `source/_post/` 目录下多了一个 `hexo-blog.md` 文件，内容如下：

    ```sh
    ---
    title: hexo-blog
    date: 2016-04-07 21:20:35
    categories: 
    tags: 
    ---
    ```

2. 写文章，修改 `hexo-blog.md` 的内容

    ```sh
    ---
    title: 使用 Hexo 搭建个人博客
    date: 2016-04-07 21:20:35
    categories: 
    -- 工具
    tags: 
    -- hexo
    -- markdown

    ## 安装
    ...
    ## 写文章
    ...
    ```

3. 生成文章

    ```sh
    $ hexo generate
    ```

    刷新浏览器就可以看到刚刚完成的文章了。
