---
title: Termux 让旧手机变生产力工具
abbrlink: 7d460e84
date: 2025-12-11 15:29:52
series: termux
categories:
  - - 笔记
tags:
  - android
  - termux
  - sshd
  - nginx
---

## 什么是 Termux？

Termux 是一款基于 Terminals 的 Android 终端模拟器和 Linux 环境，它允许用户在 Android 手机上运行一个完整的 Linux 系统。Termux 提供了一个轻量级的命令行界面，支持 bash、zsh、Python、Java、Go 等多种开发工具和编程语言，并且可以安装各种 Linux 软件包。Termux 适合用于学习 Linux 命令、进行编程开发、搭建小型服务器等场景。

## Termux 的主要功能

- **Linux 环境模拟**：提供完整的 Linux 系统，让用户可以在手机上操作命令行。
- **软件包管理**：支持通过 pkg 命令安装各种软件包，如 apt、npm、pip 等。
- **脚本开发与运行**：支持多种脚本语言，如 Bash、Python、Node.js 等。
- **网络服务搭建**：可以安装并运行如 SSH、Nginx、Apache 等服务，实现远程访问或本地网站搭建。
- **终端工具支持**：内置 Vim、Nano、SSH 客户端、MySQL、PHP 等工具。
- **跨平台兼容性**：支持在 Android 上运行 Linux 命令和工具。

## Termux 的安装与配置

Termux 是一个开源的 Android 终端模拟器，可以在 Google Play 商店或 F-Droid 上下载安装。具体步骤如下：

1. 打开你的 Android 手机的 Google Play 商店或 F-Droid。
2. 搜索 Termux。
3. 找到 Termux 应用并点击安装。

安装完成后，打开 Termux 应用，你将看到一个终端界面。首次启动时，Termux 会自动进行一些初始化设置，如安装基本的软件包和配置环境。

```bash
pkg update && pkg upgrade -y
pkg install vim git
```

## 安装 Termux 的注意事项

### 根权限问题

Termux 本身并不需要 Android 系统的 root 权限，但如果你需要安装某些需要更高权限的软件包（如 `nginx` 或 `sshd`），可能需要使用 `termux-setup-storage` 命令来获取存储权限，以便在系统存储中安装软件。

```bash
termux-setup-storage
```

此命令将允许 Termux 访问手机的存储空间，这样你就可以在 `/storage/emulated/0` 路径下安装和使用某些软件。

### 无 root 环境下的安装问题

如果你没有 root 权限，一些软件包可能无法直接安装。例如，`nginx` 在非 root 环境下可能需要使用 `termux-standalone` 版本进行安装，或者你需要手动下载并编译安装。

你可以通过以下方式安装 standalone 版本：

```bash
pkg install proot
```

然后使用 `proot` 来运行一个独立的 Linux 环境：

```bash
proot --link2symlink apt update && apt install nginx
```

### 网络连接要求

为了确保能够正常访问互联网，Termux 需要你的手机保持联网状态。如果你在安装软件包时提示连接失败，请检查你的网络设置或连接 Wi-Fi。

### 使用 `pkg` 命令前的准备

在使用 `pkg install` 命令前，建议先更新软件包列表：

```bash
pkg update
```

这可以确保你安装的是最新版本的软件包，并避免因版本过旧而导致的兼容性问题。

### 软件包的依赖问题

某些软件包在安装时可能需要依赖其他软件包。例如，安装 `nginx` 可能需要安装一些依赖项，如 `libpcre` 或 `openssl`。Termux 的包管理器会自动处理这些依赖关系，但你也可以手动安装依赖项：

```bash
pkg install libpcre openssl
```

## 安装 sshd 并实现自启动

### 安装

```bash
pkg update && pkg upgrade
pkg install openssh
```

### 配置

```bash
# 查看用户名
whoami

# 设置密码
passwd

# 查看 IP 地址（用于连接）
ifconfig
```

### 启动

```bash
sshd
```

### 使用

电脑上访问：

```bash
ssh u0_a252@192.168.1.120:8022
```

- u0_a252 为用户名
- 192.168.1.120 手机 IP 地址
- 8022 为 ssh 端口

### 设置自启动

```bash
echo '
# 自动启动 SSH 服务
if ! pgrep sshd > /dev/null; then
    echo "正在启动 SSH 服务..."
    sshd
    echo "SSH 服务启动完成"
    echo "用户: $(whoami)"
    echo "端口: 8022"
    IP=$(ifconfig 2>/dev/null | grep "inet " | grep -v 127.0.0.1 | awk '{print $2}' | head -1)
    [ -n "$IP" ] && echo "IP: $IP" || echo "IP: (请连接网络后查看)"
fi
' >> ~/.bashrc
```

## 安装 Nginx 并实现自启动

### 安装

```bash
pkg update && pkg upgrade
pkg install nginx
```

### 启动

```bash
nginx
```

### 使用

电脑上访问：<http://192.168.1.120:8080>

- 192.168.1.120 手机 IP 地址
- 8080 为 Nginx 端口

### 设置自启动

```bash
echo '
# 自动启动 Nginx 服务
if ! pgrep nginx > /dev/null; then
    echo "正在启动 Nginx 服务..."
    nginx
    echo "Nginx 服务启动完成"
    echo "端口: 8080"
    IP=$(ifconfig 2>/dev/null | grep "inet " | grep -v 127.0.0.1 | awk '{print $2}' | head -1)
    [ -n "$IP" ] && echo "IP: $IP" || echo "IP: (请连接网络后查看)"
fi
' >> ~/.bashrc
```
