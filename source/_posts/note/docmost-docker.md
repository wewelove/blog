---
title: 容器快速部署 Docmost 文档协作平台
abbrlink: bbdf0136
date: 2025-11-13 17:08:44
sersies: Docmost
categories:
  - [笔记]
tags:
  - Docmost
  - Docker
  - 容器
  - 文档
---

Docmost 是一款开源的 团队协作 Wiki 与文档管理工具，定位为 Confluence 和 Notion 的开源替代品，专注于提供高效、安全且可定制的知识库解决方案。

## Compose 文件配置

```yaml
services:
  docmost:
    image: docmost/docmost:latest
    container_name: docmost
    restart: unless-stopped
    environment:
      - APP_URL=http://localhost:3000
      - APP_SECRET=6426a899-9c09-4b4f-a503-2fb54de52685
      - DATABASE_URL=postgresql://docmost:qwer1234@postgres:5432/docmost?schema=public
      - REDIS_URL=redis://redis:6379
    ports:
      - 3000:3000
    network_mode: bridge
    volumes:
      - docmost:/app/data/storage
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16-alpine
    container_name: postgres
    restart: unless-stopped
    environment:
      - TZ=Asia/Shanghai
      - POSTGRES_HOST_AUTH_METHOD=trust
      - POSTGRES_PORT=5432
      - POSTGRES_USER=docmost
      - POSTGRES_PASSWORD=qwer1234
      - POSTGRES_DB=docmost
    # ports:
    #   - 5432:5432
    network_mode: bridge
    volumes:
      - postgres:/var/lib/postgresql/data

  redis:
    image: redis:7.2-alpine
    container_name: redis
    restart: unless-stopped
    environment:
      - TZ=Asia/Shanghai
    # ports:
    #   - 6379:6379
    network_mode: bridge
    volumes:
      - redis:/data

volumes:
  docmost:
  postgres:
  redis:
```

> 请根据需要修改 Postgres 的密码：qwer1234

## 启动容器

```bash
# 启动
docker-compose up -d

# 查看日志
docker-compose logs -f
```

## 访问地址

- 地址：<http://localhost:3000> 请将 `localhost` 修改为实际地址。

![工作空间](../../images/docmost-docker/20251113175722.png){.cover}

![偏好设置](../../images/docmost-docker/20251113175912.png){.cover}
