---
title: 使用 GitHub 托管自己的项目
date: 2017-04-11 17:39:33
categories:
- 开发
tags:
- git
- github
---

## 安装

### Ubuntu 系统

```bash
$ sudo apt-add-repository ppa:git-core/ppa
$ sudo apt-get update
$ sudo apt-get install git
```

如果出现 `apt-add-repository: command not found` 错误，要先执行以下:

```bash
$ sudo apt-get install python-software-properties
$ sudo apt-get install software-properties-common
```

### Windows 系统

下载 [Git for Windows](https://git-scm.com/download/win)，建议下载安装版根据提示安装。  
Windows 系统中，以下操作均在 `Git Bash` 命令窗口中执行。

## 配置

### 设置全局用户

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "youremail@gmail.com"
```

### 设置密钥

生成 `ssh` 密钥:

```bash
$ ssh-keygen -t rsa -C "youremail@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): #保存位置,回车
Enter passphrase (empty For no passphrase): #加密,不加密直接回车
Enter same passphrase again:    #加密校验
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
......
```

如果备份了 `id_rsa` 和 `id_rsa.pub` 文件，可以直接将其复制到 `~/.ssh` 目录:

```bash
$ cp ./id_rsa* ~/.ssh
```

复制后要检查密钥文件的读写权限，安全的设置如下：

```bash 
$ ls -al ~/.ssh/id_rsa*
-rw------- 1 user grp 1675 Aug 11 02:23 /home/user/.ssh/id_rsa
-rw-r--r-- 1 user grp  401 Aug 11 02:23 /home/user/.ssh/id_rsa.pub
```

把密钥添加到 `ssh-agent` 的高速缓存中:

```bash
$ ssh-add
Identity added: /home/user/.ssh/id_rsa (/home/user/.ssh/id_rsa)
```

## 使用

### 将公钥导入远程 `git` 服务站点

在 [github.com](https://github.com) 或 [gitee.com](https://gitee.com) 上申请个帐号。  
将 `id_rsa.pub` 公钥内容添加到已经申请的帐号，目录如下：

```bash  
# github.com
Settings -> SSH and GPG keys -> New GPG key  
# gitee.com
修改资料 -> SSH公钥 -> 添加公钥  
```

### 新建项目，并克隆到本地

```bash
$ git clone git@github.com:username/project.git
## 或
$ git clone git@gitee.com:username/project.git
```

### 常用命令

```bash
$ cd project
# 拉取远程或其它仓库内容合并到本地仓库
# Fetch from and integrate with another repository or a local branch
$ git pull
# 将文件内容添加到索引中
# Add file contents to the index
$ git add --all
$ git add .
$ git add <file>...
# 提交修改到本地仓库
# Record changes to the repository
$ git commit -a -m "说明"
# 将本地暂存内容更新到远程仓库
# Update remote refs along with associated objects
$ git push
```
