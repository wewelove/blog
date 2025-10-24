---
title: PHP 加密授权 - Zend Guard 篇
date: 2017-09-04 10:37:03
categories:
  - 笔记
tags:
  - php
  - zend guard
---

## 环境

- Windows10
- Zend Guard 7.0 (32 bit)
- Nginx 版 UPUPW PHP5.6 系列环境包

## 准备

- [Zend Guard 官网](https://store.roguewave.com/zend-guard/zend-guard-annual/)  
- [Zend Grund 下载](http://www.zend.com/en/products/guard/downloads)
- [Zend Guard Loader 下载](http://www.zend.com/en/products/loader/downloads)
- [UPUPW PHP5.6 下载](http://www.upupw.net/nphp56/n119.html)  
- [UPUPW PHP5.6 帮助](http://www.upupw.net/nginxhelp/)

下载 Zend Guard、Nginx 版 UPUPW PHP5.6 系列环境包、Zend Guard Loader 软件；安装 Zend Guard，将 UPUPW PHP5.6 解压到合适的目录，
将 Zend Guard Loader 解压到 UPUPW PHP5.6 下的  `PHP/ext` 目录并替换原扩展。

## 配置

打开 `UPUPW PHP5.6/PHP5/php.ini` 文件，修改如下配置：

```conf
[ZendLoader]
zend_extension ="C:\DevTools\UPUPWNP5.6\PHP5\ext\php_ZendLoader.dll"
zend_loader.enable = 1
zend_loader.disable_licensing = 0
zend_loader.obfuscation_level_support = 3
zend_loader.license_path = "C:\DevTools\UPUPWNP5.6\htdocs\guard\licenses"
```

参数 `zend_extension` 为扩展存放目录，参数 `zend_loader.license_path` 为授权文件存放目录，请根据实际情况配置。

打开 UPUPW PHP5.6 控制面板【开启全部服务】:

![zend-guard-01](/images/zend-guard-01.png)

浏览器打开 http://localhost/u.php 地址，检查 Zend Guard Loader 扩展是否正常开启：

![zend-guard-02](/images/zend-guard-02.png)

## 源码

先准备源文件 `demo/index.php`，代码如下：

```php
<?php
phpinfo();
?>
```

## 新建项目

菜单中新建项目，输入项目名称，选择输出目录：

![zend-guard-03](/images/zend-guard-03.png)

添加源代码目录：

![zend-guard-04](/images/zend-guard-04.png)

源代码配置参数：

![zend-guard-05](/images/zend-guard-05.png)

基本配置完成：

![zend-guard-06](/images/zend-guard-06.png)

配置加密授权信息：

![zend-guard-07](/images/zend-guard-07.png)

生成授权文件：

![zend-guard-08](/images/zend-guard-08.png)

加密后的文件：

![zend-guard-09](/images/zend-guard-09.png)

## 测试

将加密输出的文件拷贝到 Web 目录，浏览器中打开。
