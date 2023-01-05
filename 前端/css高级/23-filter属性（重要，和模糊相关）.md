# filter属性（重要，和模糊相关）

作用：[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS)属性 **`filter`** 将模糊或颜色偏移等图形效果应用于元素。滤镜通常用于调整图像、背景和边框的渲染。（来自MDN）



几个常用的：

1. blur(1px)：将图像模糊（最常用的应该就是这个模糊了）
2. brightness(100%)：让图像变亮或者变暗，大于100%变凉，小于100%变暗
3. contrast(200%)：对比度



示例：

```css
/* <filter-function> values */
filter: blur(5px);
filter: brightness(0.4);
filter: contrast(200%);
filter: drop-shadow(16px 16px 20px blue);
filter: grayscale(50%);
filter: hue-rotate(90deg);
filter: invert(75%);
filter: opacity(25%);
filter: saturate(30%);
filter: sepia(60%);
```

