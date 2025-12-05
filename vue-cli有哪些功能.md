## vue-cli 有哪些功能

vue-cli 是官方提供的一个快速搭建项目的脚手架。为开发者提供了创建项目，代码编译，本地调试，测试，打包构建等功能。

### vue-cli 和 vite 的区别

- vue-cli 是基于 webpack 的，功能更加全面成熟
- vite 是基于 esbuild 的，启动和热更新速度更快

### 如何自定义 webpack 配置

- 配置 configureWebpack
- 链式配置 chainWebpack

### 如何升级 vue-cli 项目

- 执行 vue upgrade
- 手动更新 package.json 中的@vue/cli-service 以及相关插件的版本
