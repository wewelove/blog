---
title: Cmder 安装与配置
abbrlink: eb9d652e
series: Cmder
categories:
  - - 笔记
tags:
  - cmder
  - terminal
  - 终端
date: 2020-02-11 11:59:14
---

## Cmder 安装

下载地址：

* [Cmder](http://cmder.net)
* [Cmder In GitHub](https://github.com/cmderdev/cmder)

从官网下载 `Cmder` 解压到任意目录即完成安装。

> Cmder 有 mini（6MB）和 full（84MD）两个版本，都是 portable 的，解压即可使用。

## 外观配置

**Font**，右键 Tab 栏空白处，弹出菜单选择 Settings，映入眼帘的就是字体设置了。建议使用字体 Input Mono、Inconsolata、Consolas、Courier New。还可以加上中文字体，"Main font" 设置下方的"Alternative font" 添加 CJK 字体，在设置 "Unicode ranges" 成 CJK 的就好了。

**Color Schemes**，同样是在 Settings 中，左侧树形菜单中选择 Features->Colors，就能来到 Scheme 设置界面。Cmder 自带的 Scheme 很丰富，也可以通过自定 Scheme，应用网络上简洁好看的风格。Github | joonro/ConEmu-Color-Themes 提供了当前流行的 Scheme 安装方式。

**Quake Style**， 开启后，Cmder 就变成了下拉式。按住 "ctrl + \`" Cmder就从屏幕上方弹出，焦点转移就收回（可修改成再次按住 "ctrl + \`" 收回）。开启 Quake Style 之后极客感很强!

## 终端设置

**Default Terminal**， 在 Startup 置项框里就可以更改默认终端，选择 Special named task，在下拉菜单中选择适合自己的终端。什么？！找不到自己满意的，还以在 Startup->Tasks 中添加新的终端，及初始化脚本（用来执行一些命令，设置环境变量、命令别名、ssh 等）。还可以为这些终端添加快捷键 HotKey，方便快速打开。Startup->Environment，能在这里为所有 Tasks 作初始化设置。

**Split window**，按住 "ctrl + shift + e" 水平分屏；按住 "ctrl + shift + o" 垂直分屏 （可以在热键设置中更改）； 点击 Tab bar 上的[+]，选择 "New Console Dialog"，里有 "New console split" 选项，即可分屏出不同类型的 Terminal 了。

**Integration**，在其中即可添加右键菜单项，推荐用 "ConEmu Here"。按住 "Register" 即可添加，但以后删除 Cmder 之前一定要记得 "Unregister" 一下。

**HotKey**，在 Keys & Macro 中即可，修改添加各种热键。在子设置项中，还可以更改复制方式。Cmder 默认设置是左键划取文本后，就自动复制了，十分方便。

## 添加右键

以管理员方式打开命令行窗口，进入 `Cmder` 安装目录，即 `Cmder.exe` 所在目录，执行以下操作。

```sh
Cmder.exe /REGISTER ALL
```

## 参考

* [Windows 下必备神器之 Cmder](http://www.jeffjade.com/2016/01/13/2016-01-13-windows-software-cmder/)
* [逆天神器 Cmder](http://bg.biedalian.com/2014/09/11/cmder.html)
