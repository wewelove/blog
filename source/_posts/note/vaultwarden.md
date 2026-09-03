---
title: Docker 一键部署 Vaultwarden，自建安全密码库
date: 2024-01-15 10:30:00
tags:
  - Vaultwarden
  - Docker
  - Bitwarden
  - 密码库
categories:
  - [笔记]
description: 一个轻量级 Bitwarden 兼容服务器，让你用一台小服务器或树莓派，搭建属于自己的密码管理中心
---

## 什么是 Vaultwarden？

Vaultwarden（原名 bitwarden_rs）是一个用 **Rust** 编写的开源密码管理服务器，完全兼容官方 Bitwarden 客户端。

简单来说：**你可以用 Bitwarden 的手机 App、浏览器插件、桌面软件，直接连到你自己的 Vaultwarden 服务器**。数据存在你自己的机器上，而不是别人的云端。

官方 Bitwarden 服务器资源消耗大，而 Vaultwarden 只需 **不到 50MB 内存** 就能跑起来，对低配服务器极其友好。

---

## 核心特性

### 全平台兼容

官方 Bitwarden 客户端无缝连接，支持：

- **iOS / Android** 原生 App

- **Chrome / Firefox / Edge** 浏览器插件

- **macOS / Windows / Linux** 桌面客户端

- Web 界面直接管理

### 企业级安全

- **端到端加密**：你的密码只有你自己能解密

- **双因素认证（2FA）**：支持 Authenticator、Email、FIDO2 WebAuthn、YubiKey、Duo

- **紧急访问**：设定信任的联系人，危急时刻可接管你的密码库

- **密码强度分析**：自动检测弱密码和重复密码

### 家庭与团队协作

- **家庭共享计划**：最多 6 人共享密码库，零额外费用

- **组织管理**：支持角色权限、分组管理、事件日志

- **密码共享**：安全地与家人分享 WiFi 密码、流媒体账号

### 其他实用功能

- **安全便签**：存储信用卡信息、软件序列号、安全问题答案

- **Send 功能**：加密发送敏感信息（可设过期时间和密码保护）

- **附件管理**：上传身份证照片、合同扫描件等加密存储

- **管理后台**：Web 界面管理注册用户、查看统计

### 极致轻量

- 内存占用 **< 50MB**

- CPU 占用几乎为零

- Docker 镜像约 **70MB**

- 树莓派 Zero 2 W 都能跑

---

## 部署指南

### 方式一：Docker 一行命令启动

```bash
docker run -d \
  --name vaultwarden \
  -v /vw-data/:/data/ \
  -p 127.0.0.1:8000:80 \
  -e DOMAIN="https://vaultwarden.yourdomain.com" \
  --restart unless-stopped \
  vaultwarden/server:latest
```

### 方式二：Docker Compose（推荐）

创建 compose.yaml：

```yml
services:  
  vaultwarden:
  image: vaultwarden/server:latest
  container_name: vaultwarden
  restart: unless-stopped
  environment:
    DOMAIN: "https://vaultwarden.yourdomain.com"
    SIGNUPS_ALLOWED: "false"
    ADMIN_TOKEN: "your_super_secure_admin_token"
  volumes:
    - ./vw-data/:/data/
  ports:
    - "127.0.0.1:8000:80"
```

启动：

```bash
docker compose up -d
```

### 关键环境变量说明

| 变量 | 说明 | 推荐值 |
| --- | --- | --- |
| DOMAIN | 你的域名（含协议） | https://vault.exame.com |
| SIGNUPS_ALLOWED | 是否开放注册 | false建建议关闭） |
| ADMIN_TOKEN | 管理后台访问令牌 | 机生成强密码 |
| SMTP_HOST | SMTP 服务器地址 用于邮件通知 | |
| SMTP_FROM | 发件人邮箱 你的邮箱地址 | |

### 配合反向代理

推荐搭配 **Nginx Proxy Manager**（我们上期刚推荐过！）启用 HTTPS：

- 在 NPM 中添加 Proxy Host
- 目标主机填 127.0.0.1:8000
- 开启 SSL，绑定你的域名
- 完成！

## 效果对比

| 维度 | 官方 Bitwarn | Vaultwarden |
| --- | --- | --- |
| 最低内存 | ~500MB | **< 50MB** |
| 最低磁盘 | ~200MB | **~70MB** |
| 每月费用 | 人（家庭版4.5） | **¥0** |
| 数据归属 | Bitwarden 公司 | **你自己** |
| 部署难度 | 无需部署 | **Docker 一键部署** |
| 客户端兼容 | 官方客户端 | **官方客户端** |

**结论：功能几乎一样，Vaultwarden 更省资源、更省钱、更安全。**

## FAQ

### Q1：数据安全吗？会不会被黑客破解？

Vaultwarden 使用的是与官方 Bitwarden 相同的加密协议，你的密码在发送到服务器之前就已经加密了。**即使服务器被攻陷，攻击者也拿不到你的明文密码**。当然，记得设置一个足够强的主密码！

### Q2：和官方 Bitwarden 有什么区别？

Vaultwarden 是 Bitwarden API 的**第三方兼容实现**，不是官方产品。功能上覆盖了 95% 以上的使用场景。唯一缺少的是官方提供的云同步服务——但这就是你自建的原因，对吧？

### Q3：可以和家人共享吗？

完全可以！Vaultwarden 支持家庭共享计划，最多 6 人共享一个组织，零费用。把家人邀请链接发给他们，他们就能用官方 App 连接你的服务器。

### Q4：升级方便吗？

非常方便，一条命令即可：

```bash
docker compose pull && docker compose up -d
```

建议升级前备份 /vw-data/ 目录。

### Q5：需要固定 IP 吗？

不需要！只要有域名 + HTTPS 就行。推荐使用 DDNS 服务（如 Cloudflare、阿里云 DDNS）配合你的家用宽带。

---

## 总结

如果你一直想尝试密码管理，但又担心把身家性命交给第三方云服务，**Vaultwarden 就是最佳折中方案**：

- ✅ 官方客户端完美兼容，迁移成本为零

- ✅ 数据掌握在自己手中，安全感拉满

- ✅ 资源消耗极低，老机器 / 树莓派都能跑

- ✅ 家庭共享免费，一杯奶茶钱都不用花

**自建密码管理器，从 Docker 一条命令开始。**

---

> 作者：聪明的小明  
> 发布：2026-08-04 02:51
