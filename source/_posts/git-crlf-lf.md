---
title: Git 中 CRLF 与 LF 的转换
date: 2021-08-10 08:54:33
categories:
  - 开发
tags: 
  - git
  - crlf
  - lf
---

## 换行符在不同的操作系统上的表示

首先要理解的一点是，对于不同的操作系统，对于换行符的表示是不一样的。也就是说当我们在编辑一个文件，在键盘上按下回车键的时候，对于不同的操作系统保存到文件中的换行符是不一样的。见下表：

```sh
CR: 表示回车\r
LF: 表示换行\n
CRLF: 表示回车换行\r\n

敲下回车键，不同的操作系统保存到文件中的值：
Windows：使用的是 CRLF ==> 即 \r\n，文件中保存的是 \r\n
Linux/Unix: 使用的是 LF ==> 即 \n，文件中保存的是 \n
MacOS: 使用的是 CR ==> 即 \r，文件中保存的是 \r
MacOS X系统：使用的是 LF ==> 即 \n，文件中保存的是 \n（MacOS X 已经改成和 Unix/Linx 一样使用 LF）
```

问题: 既然不同的操作系统，对于换行符使用不同的表示形式，如果一个团队在开发一个共同的项目，如果你使用的是 windows 系统，而你的小伙伴用的是 Mac 的话，当你们使用 git 协同开发软件时，就会出现换行符不统一的问题。

虽然对于不同的操作系统，默认的换行符的表示方法不一样，但是编辑器是可以设置在敲下回车键的时候保存的换行符是什么的，比如常用的 vscode 就可以进行设置。直接点击编辑器右下角的 LF 或者 CRLF，出现如下图所示的设置，直接选择即可。在设置完成之后，在敲回车键，保存在文件中的换行符就是你设置的（CRLF 或者是 LF，设置什么就是什么）。

![vbox-guest-additions-01](/images/git-crlf-lf.png)

## Git 会自动对换行符进行转换

Git 为了解决上面提出的问题，会自动对换行符进行转换。转换的方案有 3 种：

1. 在提交时将 CRLF 转换为 LF，在拉取（检出 checkout）时将 UNIX 换行符（LF）替换成 CRLF。（Windows 系统推荐使用，我们在 windows 上安装 git 的时候，如果一路 next，默认是使用这个方案）。

2. 在提交时将 CRLF 转换为 LF，在拉取（检出 checkout）时不进行转换。（Linux/Unix 和 MacOS 和 MacOS X 推荐使用，在 Unix 或者类 Unix 操作系统上安装 git，默认使用这种方案）。

3. 不进行转换（这种方案对于跨平台项目不推荐使用）。

可以发现，如果不使用第3种方案，那么在 Git 仓库（包括本地仓库和 GitHub 远程仓库）中保存的文件的换行符都是 LF 表示的。

## 自己指定换行符转换方案

我们自己在开发过程中，是可以修改/设置 Git 的换行符转换方案的。修改/设置的方法有 2 种。

### 通过 Git 的全局配置进行修改

设置 autocrlf 属性，在控制台直接运行如下的一条命令就可以设置了：

```sh
// 提交时转换为 LF，检出时转换为 CRLF
git config --global core.autocrlf true   

// 提交时转换为LF，检出时不转换
git config --global core.autocrlf input   

// 提交检出均不转换
git config --global core.autocrlf false
```

上述命令运行之后，会修改 `.gitconfig` 文件。

一般在项目中，为了避免项目中同时出现 CRLF 和 LF，还可以开启 safecrlf 检查。当然，如果你的项目自己定义了语法检查规则，例如使用 eslint 去约束换行符必须是 LF，那么当你的文件中出现 CRLF 的时候，eslint 会给你错误提示信息，告诉你不能包含 CRLF，这时候，不开启 safecrlf 也是可以的 （一般建议开启）。

开启方法如下第一条命令：

```sh
// 拒绝提交包含混合换行符的文件 （一般设置为 true）
git config --global core.safecrlf true   

// 允许提交包含混合换行符的文件
git config --global core.safecrlf false   

// 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

上述命令运行之后，也会修改`.gitconfig` 文件。
