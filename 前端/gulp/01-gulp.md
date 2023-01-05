# gulp



## gup简介

基于流的前端构建工具。

流是什么？

**流就是从一个源头开始一步一步经过加工，每一个步骤都要依赖上一步结果。最终给出一个完成的成品。**



## gulp的作用

对于css文件：

1. 压缩
2. 转码

对于js文件：

1. 压缩
2. 转码（ES6转ES5）

对于html文件：

1. 压缩
2. 转码

对于静态文件的处理，对于第三方文件的处理等等...





## gulp安装

**安装gulp命令行工具(安装一个启动gulp的环境)**

目的：就是可以在命令行中使用

在全局运行：

```js
npm install --global gulp
```



gulp安装检测：

```js
gulp --version
// 4.0版本出的是gulp-cli 版本号
// 3.0版本直接出gulp 3.xx.x
```



gulp卸载：

```js
npm uninstall --global gulp
```

