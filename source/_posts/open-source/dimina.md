---
title: Dimina 轻量级跨端小程序框架
abbrlink: b180356f
categories:
  - - 开源
tags:
  - 跨平台
  - 小程序
  - APP
  - Android
  - iOS
  - Harmony
  - Web
date: 2025-10-21 09:09:09
---

## 项目简介

星河小程序（以下简称 Dimina）是滴滴自研的一套轻量级跨端小程序框架，可以理解为开源版的小程序方案，致力于为开发者提供高性能、跨平台、低门槛的开发体验。

目前，Dimina 已支持 Android、iOS、Harmony 和 Web 四大平台。开发者可以将 Dimina 作为移动端跨平台开发框架，将已有小程序逻辑以独立模块方式集成到现有 App，或直接采用小程序语法进行开发，并一键打包生成独立原生 App。

> Dimina 发音为 /diːminə/，是 didi miniprogram 的缩写，旨在打造灵活、轻量的小程序跨端开发框架。

::: center
{% btn https://github.com/didi/dimina, GitHub, fab fa-github, default outline %}
{% btn https://gitee.com/didiopensource/dimina, Gitee, iconfont icon-gitee, red outline %}
{% btn https://gitee.com/didiopensource/dimina, 官网, fas fa-home, blue outline %}
{% btn https://gitee.com/didiopensource/dimina#%E4%B8%8A%E6%89%8B%E4%BD%BF%E7%94%A8, 文档, fas fa-book, green outline %}
:::

## 技术特性

- **资源离线化**: 资源本地存储减少网络请求
- **逻辑视图分离**: 独立 JS 引擎避免主线程阻塞
- **原生能力封装**: 统一 API 调用原生功能
- **页面预加载**: WebView 预热提升性能

## 平台支持

- **Android**: QuickJS + Android WebView
- **iOS**: JavaScriptCore + WKWebView
- **Harmony**: QuickJS + Harmony WebView
- **Web**: Web Worker + Browser

## 效果展示

::: left
{% btn https://didi.github.io/dimina/, 点击查看演示效果, fas fas fa-book, green outline %}
:::

| Android | iOS | Harmony |
| --- |  --- |  --- |
| ![Android](/images/dimina-android.jpg =200x) | ![iOS](/images/dimina-ios.jpg =200x) | ![Harmony](/images/dimina-harmony.jpg =200x) |

## 快速开始

1、创建小程序项目

- 使用小程序开发工具创建项目
- 配置 app.json 和页面路由

2、开发小程序页面

- 编写 WXML 模板
- 添加 WXSS 样式
- 使用 JavaScript 编写页面逻辑

3、编译打包

- 使用 DMCC 编译器 将小程序代码编译为跨端代码
- 打包星河小程序包
- 将星河小程序包放置到各平台对应目录

4、平台接入

- Android 接入说明
- iOS 接入说明
- Harmony 接入说明

5、调试与发布

- 集成 App 进行真机调试
- 打包发布到各应用商店

## 开源协议

Dimina 源码遵循 [Apache-2.0](https://gitee.com/link?target=https%3A%2F%2Fopensource.org%2Flicense%2Fapache-2-0) 协议进行分发和使用，更多详情请参见 [协议文件](https://gitee.com/didiopensource/dimina/blob/main/LICENSE)。
