---
title: Midway后端接口开发
tags:
categories:
---

# 初始化 Midway

使用 Midwayjs koa V3 项目模板，参考[快速开始](http://www.midwayjs.org/docs/quickstart)

# MongoDB 数据库

参考[MongoDB 数据库](http://www.midwayjs.org/docs/mongodb)

## 安装依赖

1. 使用命令安装模块`yarn add mongoose @typegoose/typegoose @midwayjs/typegoose@3`

2. 配置连接信息`src/config/config.default.ts`。

   ```ts
   export default {
     // ...
     mongoose: {
       client: {
         uri: "mongodb://localhost:27017/test",
         options: {
           useNewUrlParser: true,
           useUnifiedTopology: true,
           user: "***********",
           pass: "***********",
         },
       },
     },
   };
   ```

   可以将敏感信息配置在 `.env` 文件中，例如：URL、用户名、密码等。

3. 调用数据库，根据文档创建实体，并调用数据库。
