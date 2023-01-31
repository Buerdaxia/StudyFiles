# @font-face的使用

## 基本语法：

```css
@font-face {
  font-family: <YourDefineFontName>;
  src: <url> [<format>],[<source> [<format>]], *;
  [font-weight: <weight>];
  [font-style: <style>];
}
```



属性介绍：

**font-family**：载入字体的名称，之后使用的名称就是这里面的

**src**：加载字体文件[url]加载字体，可以是相对路径，可以是绝对路径，也可以是网络地址。[format]定义的字体的格式，用来帮助浏览器识别。主要取值为：【truetype(.ttf)、opentype（.otf）、truetype-aat、embedded-opentype(.eot)、svg(.svg)、woff(.woff)】



## 基本使用

示例：

```css
@font-face {
  font-family: 'DS-DIGI'; /* 重命名字体名 */
  src: url('./DS-DIGI-1.ttf'); /*引入字体文件*/
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: 'YouSheBiaoTiHei'; /* 重命名字体名 */
  src: url('./YouSheBiaoTiHei-2.ttf');
  font-weight: normal;
  font-style: normal;
}



/*使用*/
.box {
	font-family: 'DS-DIGI';
}
```





如果是在vue项目中，一般都会将上面的字体单独存放到一个文件中，然后在App.vue文件的css中引入，之后所有页面就都可以用啦

示例：

```vue
<template>
// App.vue
</template>
<script>...</script>
<style>
@import './assets/fonts/font.scss';
</style>
```

>注意：font.scss就是上面写的@font-size内容