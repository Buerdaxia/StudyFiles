# clip-path

**`clip-path`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏。

常用的四个

[`inset()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape/inset)

定义一个 inset 矩形。

[`circle()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape/circle)

定义一个圆形（使用一个半径和一个圆心位置）。

[`ellipse()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape/ellipse)

定义一个椭圆（使用两个半径和一个圆心位置）。

[`polygon()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape/polygon)

定义一个多边形（使用一个 SVG 填充规则和一组顶点）

```css
clip-path: inset(100px 50px);
clip-path: circle(50px at 0 100px);
clip-path: ellipse(50px 60px at 0 10% 20%);
/* 多边形这个最常用，参数是顺时针给出多边形各个顶点的位置*/
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
```



例如切一个三角形：

```css
div {
  background-color: #000;
	width: 100px;
	height: 100px;
	clip-path: polygon(50% 0, 100% 100%, 0 100%);
}
```



好用的网站：[Clippy — CSS clip-path maker (bennettfeely.com)](https://bennettfeely.com/clippy/)