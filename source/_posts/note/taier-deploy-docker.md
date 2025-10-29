---
title: Docker 快速部署 Taier 分布式调度系统
date: 2025-10-29 16:10:22
series: Taier
categories:
  - [笔记]
tags:
  - Docker
  - Taier
  - 调度系统
  - ETL
---

## Docker Compose 文件

复制以下内容到 `docker-compose.yml` 文件中。

```yaml
services:
  taier-db:
    image: dtopensource/taier-mysql:latest
    environment:
      MYSQL_DATABASE: taier
      MYSQL_ROOT_PASSWORD: qwer1234
      TZ: Asia/Shanghai
    ports:
      - 3306:3306
  
  taier-zk:
    image: zookeeper:3.4.9
  
  taier:
    image: dtopensource/taier:latest
    environment:
      ZK_HOST: taier-zk
      ZK_PORT: 2181
      DB_HOST: taier-db
      DB_PORT: 3306
      DB_ROOT: root
      DB_PASSWORD: qwer1234
      TZ: Asia/Shanghai
    ports:
      - 8090:8090
```

## 启动命令

```bash
docker-compose up -d
```

## 访问地址

- 地址：http://127.0.0.1:8090/taier
- 用户：admin@dtstack.com
- 密码：admin123

## 更多内容

- [Taier 官方文档](https://dtstack.github.io/Taier/docs/guides/introduction)
