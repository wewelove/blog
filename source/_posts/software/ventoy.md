---
title: Ventoy 制作可启动 U 盘的开源工具
abbrlink: 616f3066
categories:
  - - 软件
    - 系统工具
  - - 开源
tags:
  - Ventoy
  - 启动盘
date: 2025-10-30 16:02:28
---

## 软件简介

Ventoy 是一款开源免费的多合一系统安装盘/启动盘制作工具，支持 Windows 家族与 Linux 几乎所有主流发行版。它最大的好处在于，你可以在 1 个 U 盘里集成 N 多款不同类型的操作系统安装盘 (比如 Windows、WinPE、Linux)，可引导启动，并通过菜单来选择安装。

![封面](/images/ventoy.png){.cover}

::: center
{% btn https://github.com/ventoy/Ventoy, GitHub, fab fa-github, default outline %}
{% btn https://gitee.com/longpanda/Ventoy/, Gitee, iconfont icon-gitee, red outline %}
{% btn https://www.ventoy.net/cn/, 官方网站, fas fa-home, blue outline %}
{% btn https://www.ventoy.net/cn/doc_news.html, 使用手册, fas fa-book, green outline %}
:::

::: center
{% btn javascript:void(0);, '', iconfont icon-windows, default outline %}
{% btn javascript:void(0);, '', iconfont icon-linux, default outline %}
:::

## 功能特性

- 100% 开源，完全免费，国产软件
- 使用极其简单，只需 “复制粘贴”
- 快速 (拷贝文件有多快就有多快)
- 直接从 ISO 文件启动，无需解开
- 无差异支持 Legacy + UEFI 模式
- UEFI 模式支持安全启动 (Secure Boot)
- 支持超过 4GB 的 ISO 文件
- 保留 ISO 原始的启动菜单风格(Legacy & UEFI)
- 支持大部分常见操作系统, 已测试 1200+ 个 ISO 文件
- 不仅仅是启动，而是完整的安装过程
- ISO 文件支持列表模式或目录树模式显示
- 提出 "Ventoy Compatible" 概念
- 支持插件扩展
- 支持自动安装部署(1.0.09+)
- 启动过程中支持U盘设置写保护
- 不影响 U 盘日常普通使用
- 版本升级时数据不会丢失
- 无需跟随操作系统升级而升级 Ventoy

## 系列文章

{% series Ventoy %}

## 下载地址

::: download
{% btn https://www.ventoy.net/cn/download.html, 官网下载, iconfont icon-guanwang, blue outline %}
{% btn https://github.com/ventoy/Ventoy/releases, GitHub 下载, fab fa-github, outline %}
{% btn https://gitee.com/longpanda/Ventoy/releases/, Gitee 下载, iconfont icon-gitee, red outline %}
:::

## 软件授权

:::
{% btn https://www.ventoy.net/cn/donation.html, 免费 - 捐赠助力, iconfont icon-coffee, red outline %}
{% btn https://github.com/ventoy/Ventoy, 开源 - 参与贡献, iconfont icon-open-source, green outline %}
:::

## 开源协议

Ventoy 源码遵循 [GPL-3.0](https://github.com/ventoy/Ventoy?tab=GPL-3.0-1-ov-file) 协议进行分发和使用。
