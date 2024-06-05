---
title: Nuxt3安装Pinia与pinia-plugin-persistedstate
date: 2024-06-05 22:26:28
categories: 技术
tags:
  - JavaScript
  - Nuxt3
  - Pinia
---

## pinia 安装

### 安装

```bash
yarn add pinia @pinia/nuxt
# 或者使用 npm
npm i @pinia/nuxt
```

添加到 nuxt.config.ts

```js
// nuxt.config.js
export default defineNuxtConfig({
  // ... 其他配置
  modules: [
    // ...
    "@pinia/nuxt",
  ],
});
```

## pinia-plugin-persistedstate 安装

1. 安装

```bash
pnpm i -D @pinia-plugin-persistedstate/nuxt
npm i -D @pinia-plugin-persistedstate/nuxt
yarn add -D @pinia-plugin-persistedstate/nuxt
```

2. 添加到 nuxt.config.ts

```js
export default defineNuxtConfig({
  modules: [
    "@pinia/nuxt", // 需要先安装pinia
    "@pinia-plugin-persistedstate/nuxt",
  ],
});
```

## 可能遇到的问题

在 VSCode 中打开项目并安装以上包后，可能一时间无法找到依赖，代码检查会报错。
此时重新启动一下开发服务器一下就能解决，不应使用 import 去直接导入。
