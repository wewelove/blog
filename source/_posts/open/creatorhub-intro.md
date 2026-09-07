---
title: CreatorHub 简介与入手指南——多平台内容管理利器
tags:
  - CreatorHub
  - 内容采集
  - 多平台
  - FastAPI
  - 自媒体
  - 抖音
  - 小红书
categories:
  - - 开源
  - - 软件
    - 效率工具
description: 全面介绍 CreatorHub——一款支持抖音、小红书、快手、视频号的多平台内容管理开源工具，涵盖项目概述、平台能力对比、快速部署、基本使用和配置指南。
abbrlink: 36b17e57
date: 2026-09-04 01:00:00
---

## 项目概述

靠跨平台搬运短视频赚十几万的案例，不少行业内人都听说过。但还在手动一条条扒视频、导素材做矩阵，纯靠人力硬肝，效率实在太低。

CreatorHub 正是为解决这一痛点而生的开源项目。它把抖音、小红书、快手、视频号整合到同一个后台面板，实现了：

1. 批量盯博主账号，实时监控新作更新
2. 自动采集下载素材，省去手动保存
3. 多账号集中管理，实现跨平台分发
4. 评论监控消息推送，还能对接通知渠道

CreatorHub 使用 Python + FastAPI 提供统一 Web 界面，用于管理账号、监控作品与评论、下载内容、发布作品和接收通知。所有账号登录态、数据库及媒体文件均保存在本地，不依赖任何第三方云服务。

浏览器交互层面，CreatorHub 按平台分别使用系统 Chrome CDP 或开源的 Patchright，业务层统一使用兼容的 Playwright API。每个账号使用独立的浏览器 Profile，Cookie、缓存和本地存储完全隔离。

![CreatorHub 总览面板（抖音）](../../images/creatorhub-intro/01.png)

![CreatorHub 总览面板（小红书）](../../images/creatorhub-intro/02.png)

## 平台能力对比

CreatorHub 目前支持抖音、小红书、快手和视频号四个主流平台，各平台能力覆盖程度如下：

| 功能            | 抖音                      | 小红书                    | 快手                   | 视频号                 |
| --------------- | ------------------------- | ------------------------- | ---------------------- | ---------------------- |
| 登录            | 扫码 / 创作者 / Cookie    | 扫码 / 创作者             | 扫码 / 创作者          | 扫码                   |
| 关键词批量采集  | 作品 / 评论 / 媒体        | 规划中                    | —                      | —                      |
| 作品监控        | 是                        | 是（创作者 / 关键词）     | 是                     | 仅本账号               |
| 评论监控        | 是                        | 是                        | 是                     | 仅本账号               |
| 短视频弹幕监控  | 是（播放页 / 创作中心）   | —                         | —                      | —                      |
| 内容下载        | 是（可选画质）            | 是（图集 / 视频）         | 是                     | —                      |
| 发布            | 是                        | 是                        | 是                     | 是                     |
| 自动评论 / 回复 | 是                        | 是                        | 是                     | —                      |
| 本账号管理      | 作品 / 关注 / 粉丝 / 私信 | 作品 / 关注 / 粉丝 / 私信 | 作品 / 关注 / 粉丝     | 作品 / 数据 / 评论     |
| 通知渠道        | Bark / 钉钉 / Telegram    | Bark / 钉钉 / Telegram    | Bark / 钉钉 / Telegram | Bark / 钉钉 / Telegram |

> 注意：视频号只支持创作者助手中的本账号数据，不支持监控或下载他人作品。

## 快速开始

### 环境要求

| 要求                 | 说明                                            |
| -------------------- | ----------------------------------------------- |
| Python               | 3.10+                                           |
| 桌面环境             | 扫码登录时需要弹出浏览器                        |
| Google Chrome 稳定版 | 可选，但小红书扫码登录建议安装                  |
| Node.js              | 18+（仅启用小红书 API 发布兼容模式时需要）      |
| 系统 ffmpeg          | 可选；未安装时自动使用 Python 依赖附带的 ffmpeg |

### 一键启动

克隆项目：

```bash
git clone https://github.com/3441293738/creatorhub.git
cd creatorhub
```

Windows 系统：

```bat
.\start.cmd
```

macOS / Linux 系统：

```bash
chmod +x start.sh
./start.sh
```

首次运行会自动完成以下步骤：

1. 创建 Python 虚拟环境
2. 安装所有依赖
3. 安装 Chromium 浏览器
4. 生成 `config.yaml` 配置文件

然后自动打开访问地址：

```text
http://127.0.0.1:8000
```

> 小红书登录建议：尽量使用本机系统中已安装的稳定版 Google Chrome。CreatorHub 会优先通过 CDP 启动系统 Chrome，并为每个账号使用独立的持久化 Profile，不会读取或复用个人 Chrome 的日常 Profile。

### 常用命令

```bash
.\start.cmd install        # 重新安装或更新依赖
.\start.cmd check          # 环境自检
.\start.cmd --no-open      # 启动后不自动打开页面
.\start.cmd --port 8080   # 使用其他端口
.\start.cmd --reload      # 开发模式
```

macOS / Linux 将 `.\start.cmd` 换成 `./start.sh` 即可。

### 手动安装（可选）

如需手动安装，按以下步骤操作：

```bash
# 创建虚拟环境
python -m venv .venv

# Windows 激活
.venv\Scripts\activate

# macOS / Linux 激活
source .venv/bin/activate

# 安装 Python 依赖
python -m pip install -r requirements.txt

# 安装浏览器
python -m patchright install chromium

# 复制配置文件
cp config.example.yaml config.yaml

# 环境自检
python selftest.py

# 启动服务
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000
```

仅当显式启用小红书 API 发布兼容模式时，才需安装 Node.js 依赖：

```bash
npm install
```

## 基本使用

### 添加账号

1. 在顶部选择目标平台
2. 打开左侧「账号」菜单
3. 选择扫码登录、创作者登录或 Cookie 登录其中一种方式
4. 登录完成后，可在账号列表中刷新资料、检测状态或重新登录

小红书扫码登录只获取主站读取态；需要发布时，请再使用独立的「创作者登录」入口。添加笔记或创作者监控时，建议使用包含 `xsec_token` 的完整链接。

### 监控与下载

![作品监控界面](../../images/creatorhub-intro/03.png)

CreatorHub 提供了丰富的监控和下载能力：

- **关键词批量采集**：当前仅支持抖音。一次输入最多 20 个关键词，可设置每词作品数、每作品评论数、是否包含二级评论及是否下载媒体。已结束任务支持编辑配置、保留结果去重续跑和 Excel 导出。
- **作品监控**：添加创作者主页、作品链接、短链或平台 ID，发现新作品后自动入库。
- **评论监控**：可订阅单条作品，也可监控账号近期作品的评论。
- **短视频弹幕监控**：独立于评论区，按视频内时间轴渐进探测并排序。支持时间范围、关键词、文本长度、点赞数和容量上限过滤，记录持久化到 SQLite。
- **链接下载**：粘贴完整分享文案或链接，自动提取地址并下载。
- **历史内容**：新增目标默认只监控订阅后的作品，也可选择回填最近若干条。

下载支持断点续传和失败重试。抖音支持可选画质，小红书支持图集和视频。

> 注意：关键词采集是一次性批处理，与持续轮询的作品监控相互独立。搜索结果受平台排序、账号登录态和当前可见范围影响，因此作品数和评论数是采集上限，不代表平台全量数据。

### 发布与转发

![内容发布界面](../../images/creatorhub-intro/04.png)

- 小红书支持图集、视频和定时发布
- 抖音、快手和视频号通过对应创作平台发布
- 已下载的抖音作品可转发到小红书或视频号，小红书作品可转发到抖音
- 发布前可修改标题、正文和话题

### 本账号管理与通知

- 「本账号」中可同步自己的作品、关注、粉丝和私信，具体能力因平台而异
- 小红书普通账号可通过新版 Web `/chat` 同步会话与消息
- 私信自动回复支持关键词、排除词、多模板、回复冷却和最大消息年龄
- 可启用作品健康监控，对零播放、违规或下架状态发送提醒
- 通知渠道支持 Bark、钉钉和 Telegram

> 重要提示：自动评论、回复、私信及关注操作受平台风控影响，建议低频使用。

## 配置指南

首次启动会从 `config.example.yaml` 生成 `config.yaml`。大部分常用选项也可以在 Web 面板的「设置」中直接修改。

### 常用配置项

```yaml
engine:
  scan_interval_seconds: 300 # 默认轮询间隔（秒）
  monitor_initial_backfill_count: 0 # 0=只监控新作品，-1=尽可能回填
  worker_pool_size: 2 # 下载并发数
  scan_concurrency: 2 # 抓取并发数
  account_check_interval_seconds: 1800
  media_dir: ./data/media
  work_health_enabled: false

storage:
  db_path: ./data/creatorhub.db

proxies: [] # 不使用代理
# proxies:
#   - http://user:***@host:port
#   - socks5://user:pass@host:port
```

### 注意事项

- `config.yaml`、数据库、登录态和媒体文件默认不会提交到 Git
- 每个账号使用独立浏览器配置目录；如使用代理，建议为账号绑定稳定的独立代理
- 数据库字段会在启动时自动迁移，升级后通常无需删除旧数据库
- 每个账号建议使用稳定的独立代理，避免出口 IP 漂移触发风控

### 数据目录结构

```
data/
├─ creatorhub.db   # SQLite 数据库
├─ media/          # 下载的媒体文件
└─ profiles/       # 账号浏览器配置与登录态
```

备份项目前，建议一并备份 `config.yaml` 和 `data/` 目录。

## 分享链接命令行下载

除了 Web 面板，CreatorHub 也支持纯命令行操作，适合集成到自动化流程中。

只解析链接（不访问网络）：

```bash
python -m app.engine.share_downloader --links-only "完整分享文案或链接"
```

下载内容：

```bash
python -m app.engine.share_downloader "完整分享文案或链接" -o ./data/media/share -q 1080
```

## 任务队列与风控策略

Web 面板的「任务队列」统一展示采集、发布、自动评论、账号动作和下载任务，支持按平台、队列类型、状态和关键词筛选。

### 默认风控策略

CreatorHub 默认启用 `risk_control.mode: conservative`，所有平台操作统一经过持久化调度与风控检查：

- 同一账号的评论、账号动作、私信和发布共享写操作间隔与额度
- 使用同一网络出口的账号串行访问；未配置代理的账号统一归入 `direct` 出口组
- 命中 403/429/461/471、验证码或明确风控提示后，会按阶梯进入冷却
- 被额度、时段、代理状态或冷却拦下的任务保持 `pending`，服务重启后继续恢复
- 登录态失效时任务会保留并等待重新登录

### 风控中心

「风控中心」集中展示正常、冷却、渐进恢复、登录失效、代理异常和网络熔断账号，并提供触发原因、冷却截止、恢复进度和事件时间线。规则页可调整读取间隔、冷却阶梯、恢复探测、写操作额度、出口组熔断和活跃时段。

> 服务暴露到局域网时，建议设置环境变量 `CREATORHUB_ADMIN_TOKEN` 设置管理口令，保护风控规则修改接口。

## 常见问题

| 问题                                                             | 处理方式                                                                        |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| macOS 安装依赖报错 `command /usr/bin/clang++ failed with code 1` | 更新代码后删除旧的 `.venv`，再运行安装命令；安装器会先升级 pip/setuptools/wheel |
| Playwright / Patchright 启动失败或找不到浏览器                   | 运行 `python -m patchright install chromium`                                    |
| 扫码登录没有弹窗                                                 | 确认当前机器有桌面环境；抖音也可使用 Cookie 登录                                |
| Windows 下出现 Playwright 子进程错误                             | 使用单 worker 启动，不要添加 `--workers` 参数                                   |
| 抓取不到作品或评论                                               | 检查登录态、目标链接和网络状态，必要时重新登录并降低频率                        |
| 小红书链接解析失败                                               | 重新复制包含有效 `xsec_token` 的完整链接                                        |
| 仅音频仍得到 MP4，或视频没有声音 / 画质受限                      | 重新运行安装命令更新依赖；也可安装系统 ffmpeg 并加入 `PATH`                     |

仍有问题可在 GitHub 提交 Issue，并附上平台、操作步骤和服务端错误日志。

## 使用须知

CreatorHub 定位为技术学习和个人内容管理工具，不提供账号、Cookie、代理或平台数据。使用时请遵守以下几点：

1. 遵守目标平台的服务条款和使用规范
2. 尊重内容版权，未经授权不擅自搬运他人作品
3. 遵守所在地的法律法规
4. 自动评论、回复、私信等写操作建议低频使用，避免触发平台风控

---

> **引用信息**
>
> - 原文标题：推荐一个多平台内容采集开源项目
> - 原文链接：https://mp.weixin.qq.com/s/XUjNDK1twI6jO8B9ufqx2g
> - 原文作者：Ollana
> - 公众号名称：Ollana
> - GitHub 地址：https://github.com/3441293738/creatorhub
> - 在线预览：https://3441293738.github.io/creatorhub/
