---
title: NPM 包发布流程
date: 2020-06-06 15:47:02
categories:
  - [笔记]
tags: 
  - npm
  - package
  - publish
  - 包
  - 发布
---
## 注册 NPM 账号

点击 [注册账号](https://www.npmjs.com/signup)，并完成邮箱验证。

## 初始化 NPM 项目

```sh
# 创建项目目录
mkdir <project>
# 进入项目目录
cd <project>
# 初始化项目
npm init
```

执行上述命令，根据提示输入相关信息，完成后目录下会生成 `package.json` 文件，内容如下：

```json
{
  "name": "demo",
  "version": "1.0.0",
  "description": "A demo projcet of npm.",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm"
  ],
  "author": "hao",
  "license": "ISC"
}
```

- name：项目名称
- version： 版本号
- description: 项目描述
- main：包入口文件，默认为 index.js
- repository：代码存放地址（一般是 git 地址）
- keywords：项目关键字，便于搜索
- author：作者
- license：授权协议

详情请参考：[package.json](https://docs.npmjs.com/files/package.json)

## 添加用户

如果你使用的是 taobao 源，要先切换到官方源：

```sh
# npm 官方源
npm config set registry=http://registry.npmjs.org
# taobao 源
npm config set registry=http://registry.npm.taobao.org/
```

添加用户，根据提示输入账号信息：

```sh
npm adduer
```

## 发布 NPM 包

登陆，根据提示输入用户名和密码：

```sh
npm login
```

发布：

```sh
npm publish
```
