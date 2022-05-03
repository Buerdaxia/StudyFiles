# vue中使用scss

>第一步，首先安装scss和scss-loader

```js
npm install scss
npm install scss-loader
```

>第二步编写，.scss文件

创建一个`src/styles/variables.scss`

```scss
// sidebar
$menuText: #bfcbd9;
$menuActiveText: #ffffff;
$subMenuActiveText: #f4f4f5;

$menuBg: #304156;
$menuHover: #263445;

$subMenuBg: #1f2d3d;
$subMenuHover: #001528;

$sideBarWidth: 210px;
$hideSideBarWidth: 67px;
$sideBarDuration: 0.28s;

// 导出
:export {
  menuText: $menuText;
  menuActiveText: $menuActiveText;
  subMenuActiveText: $subMenuActiveText;
  menuBg: $menuBg;
  menuHover: $menuHover;
  subMenuBg: $subMenuBg;
  subMenuHover: $subMenuHover;
  sideBarWidth: $sideBarWidth;
  hideSideBarWidth: $hideSideBarWidth;
}

```

>第三步，在main.js中引入

```js
import 'variables.scss'
```

> 第四步，配置scss-loader，让所有组件全局访问这些变量

在vue.config.js配置

```js
module.exports = {
  css: {
    loaderOptions: {
      // sass-loader的配置，配置好路径可以在所有页面上使用sass变量而需要引入
      sass: {
        // additionalData: 或 prependData:   // 8版本用prependData: 
        prependData: `
          @import "@/styles/variables.scss";  // scss文件地址
          @import "@/styles/mixin.scss";     // scss文件地址
        `
      }
    }
  }
}
```



>之后可以直接在sytle标签中使用这些变量，而不需要引入