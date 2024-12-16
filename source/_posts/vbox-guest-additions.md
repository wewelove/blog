---
title: VirtualBox 实现目录共享
date: 2017-11-02 08:33:58
categories:
- 运维
tags:
- virtualbox
---

## 环境

1. VirtualBox 5.1.30
2. 主机 Windows 10
3. 虚拟机 Ubutnu Sever 16.04

## 目标

实现主机目录与虚拟机共享，虚拟机搭建测试环境，目录指向共享目录，主机中编码开发。测试环境可以在团队中共享。

## 安装 VBoxGuestAdditions

1. 准备

    安装 `kernel headers` 和 `build tools`，执行如下命令。

    ```
    # 请使用 root 用户操作
    apt-get install build-essential module-assistant
    ```

2. 安装 

    将 VBoxGuestAdditions.iso 文件解压上传到虚拟机后安装，VBoxGuestAdditions.iso 文件可以在 VirtualBox 安装目录中找到。

    ```
    # 请使用 root 用户操作
    cd VBoxGuestAdditions
    chmod a+x VBoxLinuxAdditions.run
    ./VBoxLinuxAdditions.run 
    # 安装完成后重启虚拟机
    ```

## 目录共享

1. 配置共享目录

    ![vbox-guest-additions-01](/images/vbox-guest-additions-01.png) 

2. 挂载共享目录

    虚拟机中新建 `/www/wwwroot` 目录作为挂载点。 

    ```
    mkdir -p /www/wwwroot
    ```

    将虚拟机 `/rec/rc.local` 文件下载，添加如下内容，然后上传覆盖原文件。

    ```
    mount -t vboxsf wwwroot /www/wwwroot
    exit 0
    ```

    > `wwwroot` 为上一步配置的 `共享文件夹名称`

3. 检查效果

    完成以上两步后重启虚拟机，然后看看 `/www/wwwroot` 目录与主机 `wwwroot` 目录内容是否相同。

## 异常处理

1. 软链接错误

    ```
    cd /www/wwwroot
    ln -s /home/website website
    ln: failed to create symbolic link `website': Read-only file system
    ```

    ```
    # php 项目提示如下错误
    symlink(): Read-only file system
    ```

    解决办法:

    在主机中以管理员权限执行如下命令:

    ```
    VBoxManage setextradata "VM-NAME" VBoxInternal2/SharedFoldersEnableSymlinksCreate/SHARE-FOLDER 1
    ```
    > 请将 VirtualBox 安装目录加入环境变更 Path 中  
    > `VM-NAME` 虚拟机名称，`SHARE-FOLDER` 为共享文件夹名称

    同样以管理员权限执行如下命令，以无界面方式启动/停止虚拟机：

    ```
    VBoxManage startvm "VM-NAME" --type headless
    VBoxManage controlvm "VM-NAME" poweroff
    ```


