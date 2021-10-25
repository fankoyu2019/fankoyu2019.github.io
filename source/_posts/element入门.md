---
title: 前端|element入门
tags:
 - 前端
 - element
date: 2021-02-01 8:55:00
---



**当值为数字要在属性前加:**

<!--more-->

1.新建vue-cli项目

```
vue init webpack element-ui-test 
```

2.在新项目中添加element-ui库

```
cd element-ui-test
npm install element-ui --save
```

3.在main.js中配置element-ui

```js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
//以下为新增内容
引入ElementUI
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

配置ElementUI
Vue.use(ElementUI)
//-----------------------
Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

4.启动vue

```
npm start
```



## Container

```vue
<div>
  <el-container>
    <el-header>header</el-header>
    <el-container>
    <el-aside>aside</el-aside>
    <el-main>main</el-main>
    </el-container>
    <el-footer>footer</el-footer>
  </el-container>
</div>
```



elcontainer内有el-header或者el-footer子元素，全部子元素呈垂直上下排列

需要水平左右排列时，需要再定义el-container包含水平左右排列的子元素

## Layout

通过基础的 24 分栏，迅速简便地创建布局。 

![1616894450089](C:\Users\ADMINI~1\AppData\Local\Temp\1616894450089.png)

## 表单系列

