---
title: 为 Hexo 增加流程图解析功能
date: 2017-09-06 15:44:02
series: Hexo
categories:
  - [笔记]
tags:
  - hexo
  - flowchart
---

## 安装

[hexo-filter-flowchart](https://github.com/bubkoo/hexo-filter-flowchart) 插件:

```sh
npm install --save hexo-filter-flowchart
```

## 配置

站点配置文件 `_config.yml` 中增加如下配置:

```yml
flowchart:
  raphael: https://cdn.bootcss.com/raphael/2.2.8/raphael.min.js
  flowchart: https://cdn.bootcss.com/flowchart/1.11.3/flowchart.min.js
  options: 
      scale: 1,
      line-width: 2
      line-length: 50
      text-margin: 10
      font-size: 12
```

## 示例

新建文章，增加如下内容：

```txt
st=>start: 开始:>http://www.google.com[blank]
e=>end: 结束:>http://www.google.com[blank]
op1=>operation: 操作
sub1=>subroutine: 子流程
cond=>condition: 成功?:>http://www.google.com[blank]
io=>inputoutput: 输入输出...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```
