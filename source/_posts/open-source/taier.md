---
title: Taier 分布式调度系统
date: 2025-10-29 15:45:29
categories:
  - [开源]
tags:
  - Taier
  - 任务调试
  - 数据同步
  - ETL
---

## 项目名称

Taier 是一个开源的分布式 DAG 调度系统，专注不同任务的提交和调度。旨在降低 ETL 开发成本，解决任务之间复杂的依赖关系和提交、调度、运维带来的上手成本。

在 Taier 上进行 ETL 开发，不用关心任务错综复杂的依赖关系与底层的大数据平台的架构实现，将工作的重心更多地聚焦在业务之中。

Taier 提供了一个提交、调度、运维、指标信息展示的一站式大数据开发平台。

![封面](/images/taier.png)

::: center
{% btn https://github.com/DTStack/Taier, GitHub, fab fa-github, default outline %}
{% btn https://gitee.com/dtstack_dev_0/taier, Gitee, iconfont icon-gitee, red outline %}
{% btn https://dtstack.github.io/Taier/, 官方网站, fas fa-home, blue outline %}
{% btn https://dtstack.github.io/Taier/docs/guides/introduction, 使用文档, fas fa-book, green outline %}
:::

## 技术特性

- 分布式扩展
- 可视化 DAG 配置
- IDE 式开发平台
- 自定义扩展任务插件
- 向导、脚本多种模式
- 上下游依赖调度
- 支持实时、离线任务
- 支持对接不同版本的 Hadoop
- 支持Flink Standalone
- 对集群环境 0 侵入
- 多租户多集群隔离
- 支持 Kerberos 认证
- 任务多版本支持
- 自定义参数替换
- 集群资源实时监控
- 数据指标实时获取
- 任务资源限制

## 效果展示

::: left
{% btn https://dtstack.github.io/Taier/, 官方网站, fas fas fa-book, green outline %}
:::

## 系列文章

{% series Taier %}

## 开源协议

Taier 遵循 [Apache-2.0](https://github.com/DTStack/Taier/tree/master?tab=Apache-2.0-1-ov-file) 协议进行分发和使用。
