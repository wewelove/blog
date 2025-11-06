---
title: SafeLine 国产 Web 防火墙
abbrlink: 5bcfd59b
categories:
  - [开源]
tags:
  - SafeLine
  - 安全
date: 2025-10-29 14:51:58
---

## 项目简介

SafeLine，中文名 "雷池"，是一款简单好用, 效果突出的 Web 应用防火墙(WAF)，可以保护 Web 服务不受黑客攻击。

雷池通过过滤和监控 Web 应用与互联网之间的 HTTP 流量来保护 Web 服务。可以保护 Web 服务免受 SQL 注入、XSS、 代码注入、命令注入、CRLF 注入、ldap 注入、xpath 注入、RCE、XXE、SSRF、路径遍历、后门、暴力破解、CC、爬虫 等攻击。

![封面](../../images/safeline/20251106114523.png){.cover}

::: center
{% btn https://github.com/chaitin/SafeLine, GitHub, fab fa-github, default outline %}
{% btn https://gitee.com/wjqnxw/safeline, Gitee, iconfont icon-gitee, red outline %}
{% btn https://waf-ce.chaitin.cn/, 官方网站, fas fa-home, blue outline %}
{% btn https://help.waf-ce.chaitin.cn/, 使用文档, fas fa-book, green outline %}
:::

## 技术特性

- **阻断 Web 攻击** 可以防御所有的 Web 攻击，例如 `SQL 注入`、`XSS`、`代码注入`、`操作系统命令注入`、`CRLF 注入`、`XXE`、`SSRF`、`路径遍历` 等等。
- **限制访问频率** 限制用户的访问速率，让 Web 服务免遭 `CC 攻击`、`暴力破解`、`流量激增` 和其他类型的滥用。
- **人机验证** 互联网上有来自真人用户的流量，但更多的是由爬虫, 漏洞扫描器, 蠕虫病毒, 漏洞利用程序等自动化程序发起的流量，开启雷池的人机验证功能后真人用户会被放行，恶意爬虫将会被阻断。
- **身份认证** 雷池的 "身份认证" 功能可以很好的解决 "未授权访问" 漏洞，当用户访问您的网站时，需要输入您配置的用户名和密码信息，不持有认证信息的用户将被拒之门外。
- **动态防护** 在用户浏览到的网页内容不变的情况下，将网页赋予动态特性，对 HTML 和 JavaScript 代码进行动态加密，确保每次访问时这些代码都以随机且独特的形态呈现。

## 效果展示

::: left
{% btn https://demo.waf.chaitin.com:9443/, 在线演示, fas fas fa-book, green outline %}
:::

## 系列文章

{% series SefeLine %}

## 开源协议

SafeLine 源码遵循 [GPL-3.0](https://github.com/chaitin/SafeLine/tree/main?tab=GPL-3.0-1-ov-file) 协议进行分发和使用。
