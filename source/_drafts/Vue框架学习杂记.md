---
title: Vue框架学习杂记
date: 2018-04-01 14:35:41
tags:
    - npm
    - vue
    - webpack
    - 前端
categories:
    - 计算机科学
description: Vue框架学习杂记
---

## 1. npm

### 1.1 npm install 中产生package-lock.json作用，以及和package.json的区别

​	锁住当前依赖包的版本信息以及来源等信息。在package.json更新的时候会更改package-lock.json，为了保证在依赖包版本更新的时候，项目npm install使用的是lock中相同的版本

### 1.2 npm install中—save和—save-dev区别

通过—save，安装的依赖在dependencies，通过—save-dev安装的依赖在devDependencies；
前者是指在开发结束后，已经打包得到的最终项目运行时需要的依赖，比如jQuery，webpack等；后者是指在开发过程中的所需的依赖，比如一些中间件，框架等；所以一般都是安装在--save-dev；

## 2. webpack

- webpack.config.js配置文件
  - 为什么部分路径需要用__dirname+\*？
  - 打包结果加入了哈希值，如何设置使得不用重复打包
  - output.publicPath
- html-webpack-plugin
- webpack-dev-middleware
- webpack-hot-middleware



## 3. vue

- vue组件使用，data必须是函数
- element-ui
- slot
- v-for key
- 变异方法 vs 非变异方法
- render
- vue-router router-link router-view


