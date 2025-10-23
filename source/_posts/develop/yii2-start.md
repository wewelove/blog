---
title: Yii2 框架入手
date: 2017-09-26 10:13:32
categories:
  - 软件开发
tags:
  - php
  - yii2
---

## 简介

Yii 是一个高性能，基于组件的 PHP 框架，用于快速开发现代 Web 应用程序。 名字 Yii （读作 易）在中文里有“极致简单与不断演变”两重含义， 也可看作 Yes It Is! 的缩写。

* [权威指南 - EN](http://www.yiiframework.com/doc-2.0/guide-index.html)
* [API文档 - EN](http://www.yiiframework.com/doc-2.0/index.html)
* [中文社区](http://www.yiichina.com/)

## 安装

推荐使用 `composer` 安装，如果还没有安装 `composer` 请 [移步](/2017/09/26/composer-start/)。

先安装 [Composer Asset Plugin](https://github.com/francoispluchino/composer-asset-plugin) 插件：

```sh
composer global require fxp/composer-asset-plugin
```

选择所需的 Yii2 应用模板进行安装：

```sh
# Basic 
composer create-project yiisoft/yii2-app-basic yii-basic
# Advanced
composer create-project yiisoft/yii2-app-advanced yii-advanced
```

进入项目目录并初始化：

```sh
$ ./init
Yii Application Initialization Tool v1.0

Which environment do you want the application to be initialized in?

  [0] Development
  [1] Production

  Your choice [0-1, or "q" to quit] 0

  Initialize the application under 'Development' environment? [yes|no] yes

  Start initialization ...
```

> 上面初始化项目为开发状态。

配置数据库信息，迁移数据：

```sh
# 打开配置文件
# common\config\main-local.php
'components' => [
    'db' => [
        'class' => 'yii\db\Connection',
        'dsn' => 'mysql:host=localhost;dbname=yii2advanced',
        'username' => 'root',
        'password' => 'root',
        'charset' => 'utf8',
        'tablePrefix' => 'yii_'
    ],
]
```

```sh
# 迁移数据
$ ./yii migrate
Yii Migration Tool (based on Yii v2.0.12)

Creating migration history table "yii_migration"...Done.
Total 1 new migration to be applied:
        m130524_201442_init

Apply the above migration? (yes|no) [no]:yes
*** applying m130524_201442_init
    > create table {{%user}} ... done (time: 0.020s)
*** applied m130524_201442_init (time: 0.035s)


1 migration was applied.

Migrated up successfully.
```

## 配置服务器

使用 Apache 服务器：

```conf
# 将 path/to/basic/web 修改为项目的实际目录
DocumentRoot "path/to/basic/web"

<Directory "path/to/basic/web">
    # 开启 mod_rewrite 用于美化 URL 功能的支持
    RewriteEngine on
    # 如果请求的是真实存在的文件或目录，直接访问
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    # 如果请求的不是真实文件或目录，分发请求至 index.php
    RewriteRule . index.php
</Directory>
```

或使用 Nginx 服务器：

```conf
# 将 path/to/basic/web 修改为项目的实际目录
server {
    charset utf-8;
    client_max_body_size 128M;

    listen 80; ## listen for ipv4
    #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

    server_name mysite.local;
    root        /path/to/basic/web;
    index       index.php;

    access_log  /path/to/basic/log/access.log;
    error_log   /path/to/basic/log/error.log;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass 127.0.0.1:9000;
        #fastcgi_pass unix:/var/run/php5-fpm.sock;
        try_files $uri =404;
    }

    location ~* /\. {
        deny all;
    }
}
```

## 验证测试

浏览器中 [打开](http://localhost)，如下图。

![yii2-start-01](/images/yii2-start-01.png)
