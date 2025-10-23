---
title: 安装 node-sass 正确姿势
date: 2021-08-10 09:09:27
categories:
  - 软件开发
tags:
  - node
  - sass
  - scss
  - node-sass
---

windows 下面安装 node-sass，确实令人头痛，正确姿势如下：

## 修改 NPM 镜像

查看当前设置：

```sh
# npm 命令
npm config get registry
# yarn 命令
yarn config get registry
```

修改为淘宝镜像：

```sh
# npm 命令
# npm config set registry http://registry.npm.taobao.org/
npm config set registry https://registry.npmmirror.com
# yarn 命令
# yarn config set registry http://registry.npm.taobao.org/
yarn config set registry https://registry.npmmirror.com
```

## 安装 windows 平台编译环境

需要在管理员权限下安装：

```sh
npm install -g node-gyp

# 安装 python 和 vs-build
# 此过程可能长时间没有反映，可从控制面板中检查是否安装成功
# 安装完成后要重新打开命令行窗口
npm install -g --production windows-build-tools
```

## 安装 node-sass

安装时可以指定 node-sass 镜像地址：

```sh
# npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
npm i node-sass --sass_binary_site=https://npmmirror.com/mirrors/node-sass/
```

成功！
