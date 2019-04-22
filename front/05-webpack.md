# webpack 前端打包工具

### 1. webpack 介绍

### 2. webpack 安装

```shell
$ yarn add webpack@3.10.0 --dev
```

### 3. webpack 处理的文件类型

html -> html-webpack-plugin
脚本  -> babel + babel-preset-react
样式 -> css-loader + sass-loader
图片，字体 -> url-loader + file-loader

### 4. webpack 插件

html-webpack-plugin, html 单独打包成文件
extract-text-webpack-plugin, 样式打包成单独文件
CommonsChunkPlugin, 提出通用模块

webpack-dev-server 2.9.7 为 webpack 项目提供 web 服务，
支持自动刷新，路径转发。

```shell
$ yarn add webpack-dev-server@2.9.7 --dev
```

issue: 安装 extract-text-webpack-plugin 后报错
    Tapable.plugin is deprecated. Use new API on `.hooks` instead

    [参考资料](https://www.jianshu.com/p/e00b235af1fb)  
    [参考资料](https://github.com/webpack/webpack/issues/6568)  

















