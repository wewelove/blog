---
title: Composer 入手
date: 2017-09-26 11:44:02
categories:
  - [软件开发]
tags:
  - composer
  - php
---

## 简介

* [官方](https://getcomposer.org)
* [中文网](http://www.phpcomposer.com)

## 安装

1. Linux / Unix / OSX 系统

    ```sh
    # 下载安装程序
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    # 校验安装程序，此步可以忽略
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '55d6ead61b29c7bdee5cccfb50076874187bd9f21f65d8991d46ec5cc90518f447387fb9f76ebae1fbbacf329e583e30') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    # 运行安装程序
    php composer-setup.php
    # 删除安装程序
    php -r "unlink('composer-setup.php');"
    # 全局安装
    mv composer.phar /usr/local/bin/composer
    ```

    > 如果出错请尝试使用 `sudo` 执行。

2. Windows 系统

    下载并且运行 [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)，它将安装最新版本的 Composer ，并设置好系统的环境变量，因此你可以在任何目录下直接使用 composer 命令。

## 配置

1. 修改 Packagist 镜像源

    ```sh
    # 配置只在当前项目生效
    composer config repo.packagist composer https://mirrors.aliyun.com/composer/
    # 取消当前项目配置
    composer config --unset repos.packagist
    ```

    ```sh
    # 配置全局生效
    composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
    # 取消全局配置
    composer config -g --unset repos.packagist
    ```
