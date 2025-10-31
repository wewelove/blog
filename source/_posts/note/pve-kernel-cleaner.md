---
title: PVE Kernel Cleaner 安装和配置指南
series: Proxmox-VE
categories:
  - [笔记]
tags:
  - PVE Kernel Cleaner
  - Proxmox VE
abbrlink: d329e16a
date: 2025-10-30 10:54:36
---

PVE Kernel Cleaner 是一个用于 Proxmox VE 系统的开源工具，旨在帮助用户轻松删除旧的或未使用的 PVE 内核。Proxmox VE 是一个开源的服务器虚拟化环境，随着新内核的发布，旧内核需要手动删除以释放 /boot 目录的空间。PVE Kernel Cleaner 通过自动化这一过程，简化了管理任务。

## 准备工作

在安装 PVE Kernel Cleaner 之前，请确保您的系统已经安装了以下软件包：

```sh
sudo apt-get install cron curl git
```

## 开始安装

打开终端，使用以下命令克隆 PVE Kernel Cleaner 仓库：

```sh
# 克隆仓库
git clone https://github.com/jordanhillis/pvekclean.git

# 进入克隆的目录
cd pvekclean

# 赋予脚本执行权限
chmod +x pvekclean.sh

# 运行安装脚本
./pvekclean.sh
```

## 应用示例

安装完成后，您可以根据需要配置 PVE Kernel Cleaner。以下是一些常用的配置选项：

保留内核数量: 使用 `-k` 或 `--keep` 选项指定保留的内核数量。

```sh
./pvekclean.sh -k 3
```

强制删除: 使用 `-f` 或 `--force` 选项强制删除旧内核，无需确认。

```sh
./pvekclean.sh -f
```

调度任务: 使用 `-s` 或 `--scheduler` 选项设置定期清理任务。

```sh
./pvekclean.sh -s
```

## 详细参考

- [PVE Kernel Cleaner](https://github.com/jordanhillis/pvekclean)
