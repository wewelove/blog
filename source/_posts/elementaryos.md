---
title: Elementary OS 使用心得
date: 2020-02-11 12:36:21
categories:
- 系统
tags:
- elementaryos
---

快速、开源的 Windows / macOS 替代方案。

## 安装

- [VirtualBox](https://www.virtualbox.org/)
- [Elementary OS](https://elementaryos.cn/)

## 增加第三方软件包源

安装某些软件时需要增加第三方软件包源，如 `elementary-tweaks` 系统调整工具。使用 `add-apt-repository` 要先安装以下软件包：

```sh
$ sudo apt-get install python-software-properties
$ sudo apt-get install software-properties-common
```

## 使用 `gdebi` 安装 `deb` 软件包

安装已编译二进制 `deb` 软件包时，要用到 `gdebi` 工具：

```sh
$ sudo apt-get install gdebi
```

## 安装 `fcitx` 小企鹅输入法

安装软件包及配置工具：

```sh
$ sudo apt-get install fcitx fcitx-config-gtk fcitx-pinyin 
$ sudo apt-get install fcitx-table-all
$ sudo apt-get install im-config
```

此时请重启系统，再通过 `im-config` 命令将 `fcitx` 设置为默认输入法。  

![配置输入法](http://static.oschina.net/uploads/space/2014/0420/143810_3tva_561214.png)

## 安装系统调整工具

```sh
$ sudo apt-add-repository -y  ppa:philip.scott/elementary-tweaks
$ sudo apt-get update
$ sudo apt-get install elementary-tweaks
```

安装完成后在 **系统配置** 中打开，详情请参考 [官方](http://www.elementaryos-fr.org/documentation/customisation/elementary-tweak/)

## 常用软件包

- [Google Chrome](http://www.google.cn/chrome/browser/desktop/index.html)
- [Opera](http://get.opera.com/ftp/pub/opera/desktop/)
- [Sbulime Text](http://www.sublimetext.com) 
- [Vs Code](https://code.visualstudio.com)
- [LNMP](https://lnmp.org)
- [Git](https://git-scm.com)
- [Node.js](https://nodejs.org)