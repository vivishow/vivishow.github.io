---
title: Arco Pro Midway（一）
date: 2022-03-01 17:03:54
tags: [Arco Pro, midway]
categories: code
---

前端使用 arco-design-pro 后端使用 midway

---

# 获取 Arco Pro 模板

打开 [Arco Pro](https://arco.design/vue/docs/pro/start)页面，根据提示安装 Arco Pro 模板。

安装好依赖后，启动项目。

# 登录页面

首先来到的是登录页面，点击注册按钮，没有效果。打开`src/views/login/compenents/login-form.vue`文件，没有发现注册页面，那就自己添加一个登录页面。

1. 复制`login-form.vue`命名为`register-form.vue`在登录页面的基础上重新改写。

2. 在`src/views/login/index.vue`中引入`register-form.vue`，使用 v-if 控制显示。

   ```html
   <script lang="ts" setup>
     import { ref } from "vue";
     import Footer from "@/components/footer/index.vue";
     import LoginBanner from "./components/banner.vue";
     import LoginForm from "./components/login-form.vue";
     import RegisterForm from "./components/register-form.vue";

     const showLogin = ref(true);
   </script>
   ```

   注册页面组件也添加进来。

   ```html
   <div class="content-inner">
     <LoginForm v-if="showLogin" @show-register="showLogin = false" />
     <RegisterForm v-else @show-login="showLogin = true" />
   </div>
   ```

3. 在`login-form.vue`中添加一个`@show-register`事件，在登录页面点击注册按钮时触发。

   ```html
   <script lang="ts" setup>
      .........
     import { defineEmits } from "vue";
     const emit = defineEmits(['showRegister']);
     .........
   </script>
   ```

   在注册按钮上添加 click 事件，触发`@show-register`事件。

4. 同样的，在`register-form.vue`中添加一个`@show-login`事件，在注册页面‘已有账号’按钮上触发。

这里使用了`defineEmits`事件在父子组件中通信，同时使用了 setup 语法糖，所以对原文件进行了一些改动。

# 后端接口

前端页面完成之后，目前的登录还是使用 mock，注册也没有对应的后端接口。接下来我们来实现后端接口。
