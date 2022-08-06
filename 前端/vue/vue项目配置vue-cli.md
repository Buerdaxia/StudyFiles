# vue项目配置

## 1、项目运行起来时，自动打开浏览器

---package.json

```
  "scripts": {

​    "serve": "vue-cli-service serve --open",//添加一个--open

​    "build": "vue-cli-service build",

​    "lint": "vue-cli-service lint"

  }
```



## 2、eslint校验功能关闭

在根目录下，创建一个`vue.config.js`文件并配置

```js
module.exports = {
  //关闭eslint
  lintOnSave:false
}
```



## 3、src文件夹简写方法，配置别名儿@

在根目录下创建一个`jsconfig.json`文件并配置

以后，@代表src文件夹

```js
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": [
        "src/*"
      ]
    }
  },
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

若是报错：在vscode的设置中搜索checkjs并打勾。

