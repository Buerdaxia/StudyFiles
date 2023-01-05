# scrollbar滚动条样式

## 以webkit为核心

以webkit为核心的浏览器都可以可以通过该属性设置滚动条的样式

有以下几种伪元素供我们使用

注意：**这些伪元素可以单独使用(单独使用就是整个浏览器的滚动条)**，也可以设置给某个元素使用

设置整个滚动条样式：

```css
::-webkit-scrollbar {
  ...
}

/*或者给指定元素设置滚动条*/
div::-webkit-scrollbar {
  ...
}
```



设置滚动条滑块样式：

```css
::-webkit-scrollbar-thumb {
  ...
}
```



设置滚动条上下按钮样式：

```css
::-webkit-scrollbar-button {
  ...
}
```



除滑动块以外的滚动条部分：

```css
::-webkit-scrollbar-track-piece {
  ...
}
```



横竖滚动条交接的角落区域：

```css
::-webkit-scrollbar-corner {
  
}
```





## 通用标准

通用标准好像还是在草案中，有以下两个属性提供设置：

```css
body {
  scrollbar-color: black;
  scrollbar-width: thin | auto |none;
}
```





## 滚动样式

涉及到滚动条，那我们会设计到一个滚动的效果，如果我们想要平滑滚动，例如使用锚点跳转时，希望页面是平滑滚动下来的，可以设置这个属性：

```css
html {
  scroll-behavior: smooth;
}
```

注意：一般都会把这个属性丢进根元素，`html`元素中
