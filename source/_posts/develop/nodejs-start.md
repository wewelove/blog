---
title: Node.js 之旅 - 好的开端
date: 2015-03-15 21:17:16
categories:
  - 软件开发
tags:
  - nodejs
  - npm
---

## 安装 Node.js

### Windows

[官网](https://nodejs.org/en/) 下载二进制安装包直接安装。  
其它方法参考 [官方文档](https://nodejs.org/en/download/package-manager/#windows)。

### Ubuntu

```sh
# 根据需要选择不同版本: 6.x、7.x ....
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
# 安装
$ sudo apt-get install -y nodejs
```

详细说明请参考 [官方文档](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)。

### CentOS

```sh
# 根据需要选择不同版本: 6.x、7.x ....
$ curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
$ curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
# 安装
$ yum -y install nodejs
```

详细说明请参考 [官方文档](https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora)。

### OSX

```sh
$ curl "https://nodejs.org/dist/latest/node-${VERSION:-$(wget -qO- https://nodejs.org/dist/latest/ | sed -nE 's|.*>node-(.*)\.pkg</a>.*|\1|p')}.pkg" > "$HOME/Downloads/node-latest.pkg" && sudo installer -store -pkg "$HOME/Downloads/node-latest.pkg" -target "/"
# 使用 Homebrew 安装
$ brew install node
# 或使用 MacPorts 安装
$ port install nodejs
```

详细说明请参考 [官方文档](https://nodejs.org/en/download/package-manager/#osx)。

## 包管理器 NPM

### 使用淘宝镜像

```sh
sudo npm config set registry https://registry.npm.taobao.org --global
sudo npm config set disturl https://npm.taobao.org/dist --global
```

### 配置参数

```sh
# 显示配置[所有]
npm config list [-l]
# 设置 <key> 参数值 <value> [全局]
npm config set <key> <value> [-g]
# 获取 <key> 参数
npm config get <key>
# 删除 <key>参数
npm config delete <key>
```

### 获取帮助信息

```sh
# 获取 <term> 命令的帮助信息
npm help <term> [<terms..>]
```

### 更新 NPM

Windows 系统的操作要慎重。

```sh
# 获取并进入 Node.js 安装目录
> npm config get prefix -g
# 最新版本
> npm install -g npm@latest
# 或LTS版本
> npm install -g npm@lts
```

OSX 和 Linux 系统的操作比较简单。

```sh
# 最新版本
$ sudo npm install -g npm@latest
# 或LTS版本
$ sudo npm install -g npm@lts
```

### 项目应用

```sh
# 初始化项目，创建 package.json 文件
npm init
# 全局安装 <pkg> 包
npm install -g <pkg>@<tag>
npm install -g <pkg>@<version>
# 项目安装 <pkg> 包，并写入 package.json 文件 [dependencies] 字段
npm install <pkg>@<tag> --save
npm install <pkg>@<version> --save
# 项目安装 <pkg> 包，并写入 package.json 文件 [devDependencies] 字段
npm install <pkg>@<tag> --save-dev
npm install <pkg>@<version> --save-dev
# 安装项目依赖包（package.json 文件 [dependencies] 字段内容）,
# 安装开发依赖包（package.json 文件 [devDependencies] 字段内容）
npm install
# 安装项目依赖包（package.json 文件 [dependencies] 字段内容）
npm install --production
# 删除 <pkg> 包，与 install 操作对应
npm uninstall [-g] <pkg>@<tag> [--save|--save-dev]
npm uninstall [-g] <pkg>@<version> [--save|--save-dev]
# 更新项目[全局] <pkg> 包
npm update [-g] [<pkg>...]
# 显示项目[全局]已经安装的包
npm ls [-g]
```

### 常用包

```sh
# Facebook 出品，可替代 npm 的包管理工具
$ sudo npm install -g yarn
# Node.js 版本管理，nvm 不支持 Windows 操作系统
$ sudo npm install -g nvm
```

## 第一个项目

### 初始化项目

```sh
mkdir nodejs-start
cd nodejs-start
npm init
```

根据提示完成以下操作：

```sh
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (nodejs-start) # 直接回车
version: (1.0.0) # 直接回车
description: # 直接回车
entry point: (index.js) # 直接回车
test command:  node index.js # 输入 node index.js
git repository: # 直接回车
keywords: # 直接回车
author: # 直接回车
license: (ISC) # 直接回车
About to write to /home/wh/Workspace/nodejs-start/package.json:

{
"name": "nodejs-start",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
    "test": "node index.js"
},
"author": "",
"license": "ISC"
}


Is this ok? (yes) yes  # 输入 yes
```

### 安装项目依赖包 `lodash`

[`lodash`](https://lodash.com/) 是一个功能丰富的 `javascript` 工具库。

```sh
$ npm install lodash --save
$ cat package.json 
{
    "name": "nodejs-start",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
    "test": "node index.js"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
    "lodash": "^4.17.4"
    }
}
```

### 新建 `index.js` 文件

码一下代码，不要拷贝。

```js
var _ = require('lodash');

var sayHello = _.template('Hello <%= name %>!');

console.log(sayHello({'name': 'Node.js'}));
```

### 看看我们做了什么

```sh
$ node index.js
Hello Node.js!
```

或通过 `package.json` 中 `scripts` 字段的配置执行文件：

```sh
$ npm test

> nodejs-start@1.0.0 test /home/wh/Workspace/nodejs-start
> node index.js

Hello Node.js!
```

> 坚持完成上面的每一步，直到屏幕上输出 `Hello Node.js!` 。  
> 认真阅读每一步的输出信息，会有意外的收获。
