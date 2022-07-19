# 分析`react`脚手架项目结构

## `public`文件夹：

`public`一般存储静态文件

<strong style="color: red">public/index.html</strong>：界面主要HTML

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8" />
  <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
  <!-- <link rel="icon" href="./favicon.ico" /> -->

  <!-- 用于开启理想视口，用于做移动端网页适配 -->
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <!-- 用于配置浏览器页签+地址栏的颜色(仅支持安卓手机浏览器) -->
  <meta name="theme-color" content="#000000" />

  <!-- 表述网站信息的 -->
  <meta name="description" content="Web site created using create-react-app" />

  <!-- 用于指定网页添加到手机主界面的图标 -->
  <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />

  <!-- 应用加壳时的配置文件 -->
  <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

  <title>React App</title>
</head>

<body>
  <!-- 若浏览器不支持js则展示以下内容 -->
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
</body>

</html>
```



`public/robots.txt`：爬虫规则文件（可以制定什么能爬，什么不能爬）



## `src`文件夹

<strong style="color: red">src/App.js</strong>：App组件

<strong style="color: red">src/index.js</strong>：入口文件

`src/index.css`：通用样式文件（这个文件可以丢进public文件夹，然后自己修改以下引用方式即可）

`src/reportWebVitals.js`：帮助我们做页面性能分析的，需要我们自己配置

`src/setupTests.js`：做测试用的文件

