---
title: Yaak 全新的桌面 API 调试工具
categories:
  - - 软件
    - 编程开发
  - - 开源
tags:
  - Yaak
  - API
  - 调试
abbrlink: 8686fd9e
date: 2025-10-29 14:07:21
---

## 软件简介

Yaak 是一款全新的桌面 API 调试工具，支持 REST、GraphQL、gRPC、WebSocket 和 SSE 等多种协议请求。它由 Tauri 框架开发，采用 Rust 和 React 实现，目标是成为开发者的绝佳利器。

Yaak 有着简洁高效的设计，可以轻松导入 Postman、Swagger 等工具的接口数据，不仅节省了时间，还让你快速上手。不需要云服务、不含任何广告，开源授权为 MIT 协议。

![封面](/images/yaak.png)

::: center
{% btn https://github.com/mountain-loop/yaak/, GitHub, fab fa-github, default outline %}
{% btn https://yaak.app/, 官方网站, fas fa-home, blue outline %}
{% btn https://feedback.yaak.app/help, 使用文档, fas fa-book, green outline %}
:::

::: center
{% btn javascript:void(0);, '', iconfont icon-windows, default outline %}
{% btn javascript:void(0);, '', iconfont icon-linux, default outline %}
{% btn javascript:void(0);, '', iconfont icon-macos, default outline %}
:::

## 技术特性

- **支持多种协议** Yaak 几乎涵盖了所有主流的 API 协议：REST、GraphQL、SSE、WebSocket 或 gRPC。不论你是前端、后端还是全栈开发，这些功能都特别实用图片。

- **高效导入接口** 你可以从 Postman、Insomnia、OpenAPI、Swagger 或 Curl  的接口数据，无缝进行迁移切换图片。

- **隐私第一** 没有广告、没有云端绑定，也不会收集任何用户数据。

- **安全而灵活的认证机制** 完美支持 OAuth 2.0、JWT 或 Basic Auth，也可以通过自定义插件扩展认证类型。还有加密的密钥管理，可以直接将密码等敏感信息存在操作系统中图片。

- **本地版本管理与协作** API 请求的数据可以直接存储在本地磁盘上，你能用 Git 管控所有文件。对于团队协作和不同环境的快速切换，比如从开发到生产，非常方便图片。

- **可扩展性** 插件系统强大，你可以轻松自定义 UI、认证方式或模板变量。

## 系列文章

{% series Yaak %}

## 下载地址

::: download
{% btn https://yaak.app/download, 官网下载, iconfont icon-home, blue outline %}
{% btn https://github.com/mountain-loop/yaak/releases, GitHub 下载, fab fa-github, outline %}
:::

## 软件授权

[商业软件, 使用前需获取官方授权](https://yaak.app/pricing){.btn-beautify .red}
[开源软件, 捐赠点赞合作参与贡献](https://github.com/sponsors/gschier){.btn-beautify .green}

## 开源协议

Yaak 遵循 [MIT](https://github.com/mountain-loop/yaak?tab=MIT-1-ov-file) 协议进行分发和使用。
