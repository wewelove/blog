---
title: TextEncoder 使用指南
abbrlink: b07c6ebf
series: TextEncoder
categories:
  - [笔记]
tags:
  - TextEncoder
  - 效率工具
  - 编码转换
date: 2025-10-27 20:26:23
---

如何使用 TextEncoder 修改任意文本文件的编码格式和换行符类型。请按照以下步骤操作：

## 选择文件

首先将需要处理的文件添加到程序的文件列表中。您只需将文件从任意文件夹直接拖拽到 TextEncoder 窗口即可完成添加。

![textencoder](https://s.sttmedia.com/screens/textencoder-win11-en.png)

或者，您也可以使用文件列表下方的添加文件按钮（快捷键 Ctrl+O）或扫描文件夹按钮（快捷键 Ctrl+D）来导入文件。

## 设置修改内容

接下来在 "Changes" 区域（位于程序界面右侧）配置需要执行的具体操作：

![textencoder-linebreaks](https://s.sttmedia.com/screens/textencoder-linebreaks-win11-en.png)

![textencoder-encoding](https://s.sttmedia.com/screens/textencoder-encoding-win11-en.png)

- 如需修改换行符格式，请勾选 "Line Breaks" 选项

- 如需修改文本编码，请勾选 "Encoding" 选项

- 支持同时启用两种修改类型

- 通过 "Save as" 下拉菜单选择目标格式或换行符类型

## 保存设置

最后在主窗口右下角的 "Storage Options" 中选择保存方式：

- 直接覆盖原始文件

- 转换为新文件（可设置新文件名/存储路径）

完成所有配置后，点击 "Convert" 按钮即可按照您的设置批量处理列表中的所有文件。

## 批处理版本说明

本文档针对图形界面操作。程序还提供批处理版本：通过命令行或批处理脚本调用 TextEncoder 的所有功能，适用于自动化处理场景。
