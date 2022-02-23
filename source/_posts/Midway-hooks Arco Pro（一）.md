---
title: Midway-hooks Arco Pro（一）
tags:
  - midway
  - vue
categories: code
---

# 介绍

使用[Midway Hooks](https://midwayjs.org/docs/hooks/client)全栈框架主要开发后端接口,前端使用 vue,数据库使用 mongodb.

## 安装 midway

找到 Midway Hooks 的[GitHub](https://github.com/midwayjs/hooks),找到[vue 模板](https://github.com/midwayjs/hooks/tree/main/examples/vue)进行下载开发.

也可以使用如下命令安装:
`npx degit https://github.com/midwayjs/hooks/examples/vue ./hooks-app`

推荐使用 yarn 安装,使用 pnpm 时会报错.

## 配置别名

配置别名要分别在 tsconfig.json 和 midway.config.ts 中配置

1. tsconfig.json 中添加

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
import vue from "@vitejs/plugin-vue";
import { defineConfig } from "@midwayjs/hooks-kit";
import { resolve } from "path";

function pathResolver(dir: string) {
  return resolve(process.cwd(), ".", dir);
}

export default defineConfig({
  // vite相关配置 https://vite.netease.com/docs/vite-config
  vite: {
    plugins: [vue()],
    resolve: {
      alias: [
        {
          find: "/@",
          replacement: pathResolver("src") + "/",
        },
      ],
    },
  },
});
```

## 添加依赖

- 添加 vue-router 和 arco-design

  `yarn add vue-router @arco-design/web-vue`

- 导入 arco-design 和样式

  ```ts
  import ArcoVue from "@arco-design/web-vue";
  import ArcoVueIcon from "@arco-design/web-vue/es/icon";
  import "@arco-design/web-vue/dist/arco.css";
  import "@/assets/style/global.less";

  const app = createApp(App);
  app.use(ArcoVue, {});
  app.use(ArcoVueIcon);
  ```

  assets 是直接从 arco pro 项目中复制过来，还要添加 less 依赖 `yarn add -D less`

## 添加路由

1. 在 src 目录下新建 router 文件夹，放置路由文件。

   添加 typigns.d.ts 文件

   ```ts
   import "vue-router";

   declare module "vue-router" {
     interface RouteMeta {
       // 可选参数
       roles?: string[];
       icon?: string;
       locale?: string;
       menuSelectKey?: string;
       // 必须参数
       requiresAuth: boolean;
     }
   }
   ```

   `requiresAuth` 必须参数，表示是否需要登录, 除了 login 页面，其他页面都需要登录才能访问。

2. 在 router 目录下新建 index.ts 文件，放置路由配置。新建 modules 文件夹，放置路由模块。之后直接在 moduls 文件夹下新建路由文件。

   ```ts
   router / index.ts;

   import appRoutes from "./modules";

   routes: [
     // ...
     {
       name: "root",
       path: "/",
       component: "PageLayout", // PageLayout 是页面布局组件
       children: appRoutes,
     },
     // ...
   ];

   router / modules / index.ts;

   import User from "./user";

   export default [User]; // 目前只有一个路由模块，后续继续添加
   ```

3. 404 页面

   ```ts
     {
      path: '/:pathMatch(.*)*',
      name: 'notFound',
      component: () => import('@/views/not-found/index.vue'),
    },

   ```
