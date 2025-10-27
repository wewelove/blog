---
title: NativeScript-Vue Vue3 原生 App 开发框架
date: 2025-10-15 08:50:00
categories:
  - [开源]
tags:
  - uni-app
  - vue3
  - NativeScript
---

## NativeScript-Vue

NativeScript-Vue 是一个将 Vue.js 与 NativeScript 相结合的开源框架，它允许开发者使用 Vue.js 的语法和开发模式来构建真正的原生 iOS 和 Android 移动应用程序。

与基于 WebView 的混合应用框架不同，NativeScript-Vue 应用程序不是基于 Web 技术 渲染的，而是直接使用原生 iOS 和 Android 上的本地用户界面组件。

![图片](/images/nativescript-vue-1.png)

::: center
{% btn https://github.com/nativescript-vue/nativescript-vue, GitHub, fab fa-github, default outline %}
{% btn https://nativescript-vue.org/, 官方网站, fas fa-home, blue outline %}
{% btn https://nativescript-vue.org/docs/getting-started/introduction, 使用文档, fas fa-book, green outline %}
:::

## 技术特性

- 真正的原生体验：应用性能接近原生，不受 WebView 性能限制
- 跨平台开发：使用单一代码库同时开发 iOS 和 Android 应用
- 全面的原生 API 访问：直接调用 iOS 和 Android 原生 API
- Vue.js 开发体验：使用熟悉的 Vue.js 语法、组件系统和响应式数据绑定

## 快速上手

### 环境配置

```sh
# Node ≥ 18
npm i -g nativescript
ns doctor # 按提示安装 JDK / Android Studio / Xcode
# 全部绿灯即可
```

### 创建项目

```sh
ns create myApp \
  --template @nativescript-vue/template-blank-vue3@latest
cd myApp
```

> 模板已集成 **Vite + Vue3 + TS + ESLint**

### 运行调试

```sh
# 真机 / 模拟器随你选
ns run ios
ns run android
```

保存文件 → **毫秒级 HMR**，`console.log` 直接输出到终端。

### 目录速览

```sh
myApp/
├─ app/
│  ├─ components/          // 单文件 .vue
│  ├─ app.ts               // createApp()
│  └─ stores/              // Pinia 状态库
├─ App_Resources/
└─ vite.config.ts          // 已配置 nativescript-vue-vite-plugin
```

### 打包上线

```sh
ns build android --release   # 生成 .aab / .apk
ns build ios --release       # 生成 .ipa
```

`签名`、`渠道`、`自动版本号`——**标准原生流程**，CI 友好。

## Vue3 生态兼容性

| 插件                         | 是否可用 | 说明                                               |
| -------------------------- | ---- | ------------------------------------------------ |
| **Pinia**                  | ✅    | 零改动，`app.use(createPinia())`                     |
| **VueUse**                 | ⚠️   | 仅**无 DOM** 的 Utilities 可用                        |
| **vue-i18n** 9.x           | ✅    | 实测正常                                             |
| **Vue Router**             | ❌    | 官方推荐用 **NativeScript 帧导航** → `$navigateTo(Page)` |
| **Vuetify / Element Plus** | ❌    | 依赖 CSS & DOM，无法渲染                                |

检测小技巧：

```sh
npm i xxx
grep -r "document\|window\|HTMLElement" node_modules/xxx || echo "大概率安全"
```

## 调试神器

**NativeScript-Vue** 已提供 **官方 DevTools 插件**

- `组件树`、`Props`、`Events`、`Pinia` 状态 **实时查看**
- 沿用桌面端调试习惯，**无需额外学习成本**

👉 配置指南：`https://nativescript-vue.org/docs/essentials/vue-devtools`

## 插件生态

- **700+** `NativeScript` 官方插件 `ns plugin add @nativescript/camera | bluetooth | sqlite...`

- **iOS/Android SDK 直接引入** `CocoaPods` / `Maven` 一行配置即可：

    ```js
     // 调用原生 CoreBluetooth
     import { CBCentralManager } from '@nativescript/core'
    ```

- **自定义 View & 动画** 注册即可在 `<template>` 使用，**与 React Native 造组件体验一致**。

## 直达资源

- **官网 & 文档**：`https://nativescript-vue.org`
- **插件兼容列表**：`https://nativescript-vue.org/docs/essentials/vue-plugins`
- **DevTools 配置**：`https://nativescript-vue.org/docs/essentials/vue-devtools`
- **环境安装指南**：`https://docs.nativescript.org/setup/`

## 开源协议

NativeScript-Vue 遵循 [MIT](https://github.com/nativescript-vue/nativescript-vue?tab=MIT-1-ov-file) 协议进行分发和使用。
