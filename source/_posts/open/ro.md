---
title: Ro 基于洛雪音乐二次开发的音乐下载服务
categories:
  - - 开源
tags:
  - 'Ro, 洛雪, 音乐, NAS'
published: true
abbrlink: ca6962f5
date: 2026-08-11 10:20:56
---

## Ro 是什么？

Ro 是基于 **洛雪音乐（lx-music）** 二次开发的 **Headless 音乐下载服务**，专为 NAS 场景设计：

- **无头运行**：不需要图形界面，Docker 一键部署，跑在 NAS 上 7×24 小时在线
- **兼容 lx-music 音源生态**：你熟悉的 lx-music 音源脚本，直接导入就能用
- **超轻量**：内存仅约 198MB，不会挤占 NAS 上其他服务的资源
- **不内置任何音源**：Ro 只是下载框架，音源由用户自行导入，框架与音源完全解耦

> 重要说明：Ro 不内置、不提供、不托管任何音源。音源脚本需用户自行获取并导入，Ro 仅作为下载引擎与管理框架运行。请在法律允许的范围内使用。

## 核心特性一览

### 聚合搜索 · 音源平台决定

Ro 支持多维度搜索（歌曲名 / 歌手 / 专辑 / 歌单），具体能搜哪些平台、支持哪些音质，完全取决于你导入的音源脚本。既可单音源精准搜索，也可一键聚合搜索，所有音源的结果一目了然。

因为兼容 lx-music 音源格式，你之前用过的音源脚本，直接丢进来就能跑。

### 下载全链路 · 元数据完美嵌入

不只是下载音频文件——Ro 会自动将 **歌词 + 封面** 嵌入到 FLAC / MP3 文件中。下载完成后，你的音乐文件就是完整的、自带封面和歌词的完美档案。直接丢进 Plex / Jellyfin / Emby / Navidrome 等音乐管理工具，封面歌词一步到位，无需二次整理。

### 跨音源换源兜底 · 单个音源挂了也不怕

这是 Ro 最硬核的能力之一。当主音源音质降级链（flac24bit → flac → 320k → 128k）全部失败时，Ro 会自动在其他已导入的音源中匹配同一首歌，逐候选音源重试。换源后：

- 歌词/封面走实际命中音源
- 文件标题/歌手/专辑仍用原曲信息
- 任务标记为 `completed_with_warnings`，透明告知

NAS 7×24 小时在线，挂了也能自动兜底，睡一觉起来歌全下好了。

### 歌单管理 · 批量下载

支持本地歌单管理，更支持整单批量下载——找到心仪的歌单，一键下载全部。NAS 后台默默跑着，不用守在电脑前。

### 实时进度 · SSE 推送

下载任务状态通过 SSE（Server-Sent Events）实时推送，进度一目了然。手机上刷着推送，NAS 上默默下载，随时随地掌控进度。

### 健康冒烟测试 · 自动告警

NAS 上跑着的服务，最怕的就是静默失效。Ro 定时跑真实下载链路，生成 绿/黄/红 音源×音质矩阵。连续失败达阈值时，通过 Bark / Server酱 自动推送告警到你的手机。

音源挂了？你比谁都先知道。

### 超轻量 · 内存仅约 198MB

用 SQLite + p-queue 替代 Redis/BullMQ，去掉所有外部依赖。实测稳定 RSS ≈ 198MB，目标 <300MB。NAS 上跑着 Jellyfin、Nextcloud、Immich 一堆服务？放心，Ro 不会跟你抢资源。

## 技术架构

```text
ro/
├── server/            # Fastify + TypeScript 后端
│   └── src/
│       ├── core/      # 下载引擎 / 音源适配器 / 搜索 / 下载 / 换源 / 数据库 / 告警 / 冒烟
│       └── routes/    # REST API 路由
├── web/               # 原生 HTML/CSS/JS 后台（6 页）
├── data/              # 运行数据（下载 / 音源 / SQLite）
├── config.yaml        # 一份配置搞定
└── Dockerfile         # 多阶段构建
```

### 关键设计决策

| 决策 | 说明 |
| --- | --- |
| 兼容 lx-music 音源格式 | 你熟悉的音源脚本直接用，零学习成本 |
| node:vm 双层沙箱 | 隔离音源脚本，安全又轻量，无需本地 C++ 编译 |
| SQLite + p-queue | 替代 Redis/BullMQ，零外部依赖，内存 <300MB |
| 原生模块容器内编译 | better-sqlite3 / sharp 在构建阶段编译，运行阶段用 slim 镜像 |
| Session 存内存 | 重启即失效，局域网自用足够简单 |

## 音源管理——Ro 与音源完全解耦

这是 Ro 最核心的设计理念之一：**Ro 是框架，音源是插件**。

Ro 不内置、不提供、不托管任何音源脚本。用户需要自行获取 lx-music 格式的音源脚本（.js），通过以下方式导入：

1. **Web 后台上传**：音源管理页直接上传文件或在线 URL 导入
2. **直接放文件**：丢进 `data/sources/`，开启热重载自动加载
3. **API 导入**：通过接口粘贴脚本内容

导入后可查看每个音源支持的平台与音质，随时启停、热重载、删除。

> 这种设计意味着：Ro 本身不涉及任何版权内容，它只是一个通用的下载管理框架。音源的合法性由用户自行负责。

## NAS 部署，5 分钟搞定

### 方式一：Docker 镜像（NAS 玩家最推荐）

群晖 / 威联通 / Unraid / 绿联……不管你用什么 NAS，只要支持 Docker，就能跑：

```bash
# 1. 准备目录与配置
mkdir -p ro/data/downloads ro/data/sources ro/data/db && cd ro
curl -fsSL https://raw.githubusercontent.com/leizi914599611-boop/ro/main/config.example.yaml -o config.yaml
# 编辑 config.yaml，设置登录密码

# 2. 一键启动
docker run -d --name ro \
  --restart unless-stopped \
  -p 23330:23330 \
  -e TZ=Asia/Shanghai \
  -v "$PWD/data/downloads:/app/data/downloads" \
  -v "$PWD/data/db:/app/data/db" \
  --memory 512m \
  a914599611/ro-music:latest
```

打开浏览器访问 `http://<NAS的IP>:23330/`，登录即可使用！

> 支持 linux/amd64 和 linux/arm64 双架构，群晖、威联通、树莓派、Apple Silicon 都能跑。
>
> 群晖用户提示：直接在 Container Manager 中添加镜像 `a914599611/ro-music:latest`，按上述映射配置端口和目录即可。

### 方式二：Docker Compose 从源码构建

适合想自己改代码的同学：

```bash
git clone https://github.com/leizi914599611-boop/ro.git && cd ro
cp config.example.yaml config.yaml
# 编辑 config.yaml
docker compose build && docker compose up -d
```

### 方式三：本地部署（Node.js）

不用 Docker 也能跑，需要 Node.js >= 20：

```bash
git clone https://github.com/leizi914599611-boop/ro.git && cd ro
cp config.example.yaml config.yaml
cd server && npm install && npm run build && npm start
```

## 配置亮点

一份 config.yaml 管所有：

```yaml
auth:
  enabled: true
  webLogin:
    username: admin
    password: "你的密码"   # 必设！空密码禁止登录
download:
  defaultQuality: flac     # flac24bit | flac | 320k | 128k
  embedCover: true         # 自动嵌入封面
  embedLyric: true         # 自动嵌入歌词
  concurrency: 3           # 并发下载数
smokeTest:
  enabled: true
  cron: "0 6 * * *"        # 每天 06:00 冒烟
  keyword: 周杰伦
alert:
  bark:
    enabled: true
    deviceKey: "你的key"
```

## 安全设计

- **双通道鉴权**：Web 登录（内存 Session）+ API Key（给脚本/自动化用）
- **空密码禁止登录**：首次部署必须设置密码
- **密钥脱敏**：API 读取配置时只回传布尔值，绝不回明文
- **API Key 一次明文**：生成时仅展示一次，之后只能重新生成
- **限流保护**：内置固定窗口限流，防止滥用

## REST API 全覆盖

Ro 提供了完整的 REST API，所有功能均可通过接口调用：

| 功能 | 接口 |
| --- | --- |
| 搜索 | `/api/v1/search?keyword=晴天&source=xxx` |
| 聚合搜索 | `/api/v1/search/aggregate?keyword=晴天` |
| 提交下载 | `POST /api/v1/download` |
| 批量下载 | `POST /api/v1/download/batch` |
| 歌单操作 | `/api/v1/playlists` |
| 音源管理 | `/api/v1/sources` |
| 健康检测 | `/api/v1/health/smoke` |
| SSE 订阅 | `GET /api/v1/sse/subscribe` |

NAS 上跑着 Home Assistant？通过 API 联动，语音说一句"下载周杰伦的晴天"，歌就自动下好了。

## 典型 NAS 使用场景

### 场景一：NAS 音乐库一条龙

```text
Ro 下载（歌词+封面嵌入）→ NAS 共享目录 → Plex/Jellyfin/Navidrome 自动扫描 → 全屋播放
```

下载即整理，无需手动修改标签、补封面，NAS 音乐库搭建从"体力活"变成"一键活"。

### 场景二：手机远程控制下载

出门在外，想下载一首歌？手机浏览器打开 Ro 后台，搜索、下载，NAS 上默默跑着。回家就是完整的音乐库。

### 场景三：批量歌单迁移

有个歌单想全部下载？Ro 支持歌单搜索 + 批量下载，加上换源兜底，一觉醒来整单搞定。

## 重要声明

- Ro 基于**洛雪音乐（lx-music）**二次开发，不内置、不提供、不托管任何音源
- 音源脚本需用户自行获取并导入，Ro 仅作为下载引擎与管理框架运行
- Ro 定位为纯个人自用/局域网部署，不推荐直接暴露公网
- 如需公网访问，务必做好安全加固：强密码 + HTTPS 反代 + 防火墙 + 限流收紧
- 请在法律允许的范围内使用，因使用不当产生的法律责任由用户自行承担
- 项目开源协议：**Apache-2.0**

## 总结

| 维度 | 评价 |
| --- | --- |
| 易用性 | ★★★★★ Docker 一键部署，Web 后台操作 |
| NAS 友好 | ★★★★★ 轻量低内存，双架构支持，7×24 稳定运行 |
| 音源兼容 | ★★★★★ 兼容 lx-music 音源生态，零学习成本 |
| 功能性 | ★★★★★ 聚合搜索、换源兜底、歌词封面嵌入 |
| 可靠性 | ★★★★ 冒烟测试 + 告警，自动换源兜底 |

> Ro = 洛雪音乐的音源生态 + NAS 友好的无头部署 + 轻量下载框架。不臃肿、不复杂，与音源完全解耦，该有的功能一个不少，不该有的依赖一个没有。NAS 玩家的音乐下载方案，不妨试试！

## 相关链接

- 项目地址：<https://github.com/leizi914599611-boop/ro>
- Docker Hub：<https://hub.docker.com/r/a914599611/ro-music>

---
> 来源：微信公众号「Nas不无聊」· 作者：磊子 · 2026年8月9日
> 链接：<https://mp.weixin.qq.com/s/jCsmQGBHcOp8n1DcoDpSKg>
