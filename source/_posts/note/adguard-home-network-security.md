---
title: 这款开源工具，让你全屋所有设备彻底告别弹窗广告
date: 2024-01-15 10:30:00
tags:
  - AdGuard Home
  - DNS
  - 广告拦截
  - 开源
  - 网络安全
categories:
  - [笔记]
description: 介绍 AdGuard Home——一款基于 DNS 级别的开源广告拦截与网络安全工具，通过 Docker 部署即可实现全屋设备零感知净网。
---

在小说阅读器读本章，在公众号小说中沉浸阅读——这是微信公众号的体验功能，与文章正文无关，不再展开。

## 什么是 AdGuard Home？

AdGuard Home 是一款用 Go 语言编写的开源、全功能、自托管的 DNS 级广告拦截与网络安全中枢。

它的设计哲学是"从网络最源头（DNS 解析阶段）掐断恶意流量"。一旦部署在软路由、家庭 NAS 或 VPS 上，它就能接管局域网内所有设备的 DNS 请求，直接在域名解析层面把广告、隐私追踪器与恶意钓鱼域名彻底"黑洞化"，实现全屋设备零感知净网。

## 核心功能

### 1. 全屋设备免装客户端，源头毫秒级拦截

电视盒子、智能冰箱、手机 App 以及无法安装插件的 iOS 系统，只要连接 Wi-Fi，广告在解析阶段就会被直接丢弃。

### 2. 原生支持 DoH / DoT / DoQ 加密协议

彻底告别传统的明文 UDP 53 端口 DNS，全面支持 DNS-over-HTTPS (DoH)、DNS-over-TLS (DoT) 以及 DNS-over-QUIC (DoQ)。

### 3. 灵活强大的自定义规则与黑白名单生态

原生兼容 Adblock 语法规则，支持一键订阅全网主流开源广告过滤规则库。

### 4. 高颜值可视化查询日志与客户端行为分析

自带现代响应式 Web 控制台，清晰展示每台设备的请求总量、拦截率、平均响应时间及被拦截的恶意域名排行榜。

## 三步搞定全自动部署与接管

### 步骤一：使用 Docker Compose 一键启动

在服务器或 NAS 上创建并编写 `docker-compose.yml` 文件：

```yaml
version: '3.8'

services:
  adguardhome:
    image: adguard/adguardhome:latest
    container_name: adguardhome
    restart: always
    ports:
      - "53:53/tcp"    # DNS 核心协议端口
      - "53:53/udp"
      - "3000:3000/tcp"  # 初始配置端口
      - "80:80/tcp"    # Web 管理面板
    volumes:
      - ./workdir:/opt/adguardhome/work
      - ./confdir:/opt/adguardhome/conf
```

执行命令拉起服务：

```bash
docker compose up -d
```

### 步骤二：通过向导完成初始化配置

1. 浏览器访问 `http://<服务器IP>:3000` 启动初始配置向导
2. 将**管理接口**设置为监听所有网卡的 80 端口，**DNS 服务器**设置为 53 端口
3. 创建管理员账号与密码并完成保存
4. 登录控制台，进入**「设置 -> DNS 设置」**，在上游 DNS 中填入国内快速的公共 DNS（如阿里 DNS、腾讯 DNSPod）：

```text
https://dns.alidns.com/dns-query
https://doh.pub/dns-query
```

### 步骤三：添加广告过滤规则并接管全屋 DNS

1. 进入**「过滤器 -> DNS 封锁清单」**，点击**添加阻止清单**，填入开源优质过滤规则（如 anti-AD 等常用规则 URL）
2. 打开家庭主路由器后台，将 DHCP 局域网设置中的**主 DNS 服务器**修改为你部署 AdGuard Home 的服务器 IP
3. 保存路由器配置并重启手机 Wi-Fi

配置生效后，刷新手机 App 或浏览网页，AdGuard Home 控制台大屏将实时刷新出拦截图表，全屋设备瞬间进入纯净无广告世界！

## 进阶优化与并发加速建议

在**「DNS 设置 -> 缓存配置」**中，可将 DNS 缓存大小适当调大（如设为 `67108864` 即 64MB），并开启**「乐观缓存（Optimistic Caching）」**：

```bash
# 允许在后台异步刷新过期条目，让后续相同的 DNS 请求直接在 0 毫秒内从本地内存极速返回！
```

## 知行合一

干净、纯粹、安全的网络体验，不应该被铺天盖地的流氓弹窗与无休止的追踪打碎。

AdGuard Home 凭借极致优雅的 DNS 级切入点和强大的加密协议支持，为家庭和企业筑起了一道坚不可摧的网络安全防线。花几分钟部署一套属于自己的私有 DNS，把真正清爽的网络环境还给自己吧！

## 相关链接

- GitHub 项目：[AdguardTeam/AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)

---

> **引用信息**
>
> - 原文标题：这款开源工具，让你全屋所有设备彻底告别弹窗广告
> - 原文链接：https://mp.weixin.qq.com/s/KuHfmLzyias4-THrRMreZg
> - 原文作者：阿浮
> - 公众号名称：网域极客（GEEK-NET）
> - 抓取日期：2026-09-03
