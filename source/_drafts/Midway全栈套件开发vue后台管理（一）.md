---
title: Midway全栈套件开发vue后台管理（一）
tags: 
- midway
- vue
categories: code
---

# 介绍
使用[Midway Hooks](https://midwayjs.org/docs/hooks/client)全栈框架主要开发后端借口,前端使用vue,数据库使用mongodb.

## 安装
找到Midway Hooks的[GitHub](https://github.com/midwayjs/hooks),找到[vue模板](https://github.com/midwayjs/hooks/tree/main/examples/vue)进行下载开发.

也可以使用如下命令安装:
`npx degit https://github.com/midwayjs/hooks/examples/vue ./hooks-app`

推荐使用yarn安装,使用pnpm时会报错.

## 配置别名

配置别名要分别在tsconfig.json 和 midway.config.ts中配置

1. tsconfig.json中添加
  ```json
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "/@/*": ["src/*"]
    }
  }
  ```

2. midway.config.ts
  ```ts
  import vue from '@vitejs/plugin-vue';
  import { defineConfig } from '@midwayjs/hooks-kit';
  import { resolve } from 'path';

  function pathResolver(dir: string) {
    return resolve(process.cwd(), '.', dir);
  }

  export default defineConfig({
    // vite相关配置 https://vite.netease.com/docs/vite-config
    vite: {
      plugins: [vue()],
      resolve: {
        alias: [
          {
            find: '/@',
            replacement: pathResolver('src') + '/',
          },
        ],
      },
    },
  });
  ```

  这里是添加在vite部分，可以看出还是使用了vite来进行打包。

## 添加依赖

  - 添加vue-router和ElementPlus
 
    `yarn add vue-router element-plus`

  - 配置ElementPlus自动导入

    `yarn add -D unplugin-auto-import unplugin-vue-components`

    具体配置参照[ElementPlus官方文档](https://element-plus.gitee.io/zh-CN/guide/quickstart.html#%E6%8C%89%E9%9C%80%E5%AF%BC%E5%85%A5)
