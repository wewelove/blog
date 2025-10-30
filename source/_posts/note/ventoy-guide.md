---
title: Ventoy 使用指南
series: Ventoy
categories:
  - - 笔记
tags:
  - Ventoy
  - 启动盘
abbrlink: 688aa4c2
date: 2025-10-30 16:27:15
---

## 准备工作

### 硬件要求

- USB闪存盘：容量至少 8GB（推荐 32GB 以上以容纳多个系统镜像）
- 计算机：具备 USB 接口，支持从 USB 设备启动

### 软件环境

- 操作系统：Windows 7/8/10/11 或 Linux 发行版
- 管理员权限：安装过程需要系统管理员权限
- 网络连接：用于下载 Ventoy 安装包与系统镜像

### 镜像文件准备

Ventoy 支持绝大多数操作系统镜像，包括但不限于：

- Windows 系列：Windows 7/8/10/11、Windows Server
- Linux 发行版：Ubuntu、CentOS、Debian、Fedora 等
- 工具镜像：CloneZilla、SystemRescueCD、MemTest86 等
- 特殊系统：Android-x86、ChromeOS、VMware ESXi 等

完整支持列表可访问 Ventoy 官方网站查询，目前已测试超过 1200 种 ISO 文件。

## 安装步骤

下载 Ventoy 安装包 访问 Ventoy 官方 [GitHub 仓库](https://github.com/ventoy/Ventoy/releasesy)，下载最新版本的 Windows 安装包。

解压安装包 将下载的 zip 文件解压到本地文件夹，解压后会看到以下主要文件：

- Ventoy2Disk.exe：图形界面安装程序
- Ventoy2Disk_X64.exe：64 位系统专用版本
- Ventoy2Disk_ARM64.exe：ARM 架构专用版本

运行安装程序 双击 Ventoy2Disk.exe，程序会自动检测连接的 USB 设备。选择目标 U 盘，注意：安装过程会清除 U 盘数据，请提前备份。

配置安装选项:

- 分区样式：根据主板类型选择MBR或GPT（新型电脑推荐GPT）
- 设备类型：选择USB设备
- 集群大小：默认即可，大U盘可适当增大

执行安装：

点击 "安装" 按钮，确认警告信息后开始安装过程。安装完成后，U 盘会被分为两个分区：

- 小分区（约300MB）：Ventoy系统文件
- 大分区（剩余空间）：用于存放ISO镜像文件

## 镜像文件管理

### 添加系统镜像

Ventoy 使用极其简单的文件管理方式：

- 将安装好 Ventoy 的 U 盘插入电脑
- 系统会识别出一个名为 "Ventoy" 的分区（通常为 NTFS 或 exFAT 格式）
- 直接将 ISO/WIM/IMG 等镜像文件复制到该分区根目录或子目录
- 无需任何额外操作，启动时 Ventoy 会自动扫描并列出所有镜像文件
- 提示：可以创建子文件夹对镜像文件进行分类管理，如 "Windows"、"Linux"、"Tools" 等文件夹

### 支持的镜像格式

Ventoy 支持多种镜像文件格式：

- ISO：大多数操作系统安装镜像
- WIM：Windows 安装镜像格式
- IMG：磁盘镜像文件
- VHD(x)：虚拟硬盘文件
- EFI：EFI 应用程序

### 镜像文件验证

为确保镜像文件完整性，建议验证文件哈希值：

```sh
# Linux 系统示例
sha256sum ubuntu-22.04.1-desktop-amd64.iso
```

## 启动与使用

### 设置从 USB 启动

进入 BIOS/UEFI 设置：

- 开机时根据主板品牌按下对应的按键（通常为 Del、F2、F1、F12 等）
- 禁用 Secure Boot（部分系统需要）
- 设置 USB 设备为首选启动项

选择启动模式：

- Legacy BIOS：传统启动模式，适用于老旧电脑
- UEFI：新型统一可扩展固件接口，支持更大硬盘和安全启动

### Ventoy 启动菜单

成功从 U 盘启动后，将看到 Ventoy 的启动菜单界面，主要包含以下元素：

- 镜像文件列表：按字母顺序排列的所有镜像文件
- 功能菜单：F2 键进入设置，F3键切换显示语言
- 搜索功能：按 "/" 键可搜索镜像文件
- 分区信息：显示 U 盘分区使用情况

## 参考

[最完整 Ventoy 使用指南：从下载到制作启动盘一步到位](https://blog.csdn.net/gitblog_00699/article/details/151470204)
