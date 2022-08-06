# SVG

SVG 是使用 XML 来描述二维图形和绘图程序的语言。

优势：（来自菜鸟教程）菜鸟永远的神

- SVG 可被非常多的工具读取和修改（比如记事本）
- SVG 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大
- SVG 图像中的文本是可选的，同时也是可搜索的（很适合制作地图）
- SVG 可以与 Java 技术一起运行
- SVG 是开放的标准
- SVG 文件是纯粹的 XM



## 使用

SVG可以直接嵌入到HTML文件中

```html
<svg>
	<circle cx="100" cy="100" r="40" stroke="black"></circle>
</svg>
```



SVG有一些预定义的形状元素，可被开发者使用和操作：

- 矩形 <rect>
- 圆形 <circle>
- 椭圆 <ellipse>
- 线 <line>
- 折线 <polyline>
- 多边形 <polygon>
- 路径 <path>



## 一、矩形

矩形用标签`<rect>`来创建。

属性：

* width：宽度
* height：高度
* stroke：和边框有关，边框颜色
* stroke-width：边框宽度
* ...

一个黑色的矩形：

```html
<svg>
	<rect width="200px" height="100px" stroke="black"></rect>
</svg>
```



## 二、圆形

`<circle>`标签可以用来创建一个圆：

属性：

* cx：圆点x坐标
* cy：圆点y轴坐标
* r：圆的半径

一个红色的圆：

```html
<svg>
	<circle cx="100" cy="100" r="40" style="fill:red"></circle>
</svg>
```



## 三、直线

`<line>`元素用来创建一个直线

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <line x1="0" y1="0" x2="200" y2="200"
  style="stroke:rgb(255,0,0);stroke-width:2"/>
</svg>
```

属性：

* x1：在x轴定义线条的开始
* y1：在y轴定义线条的开始
* x2：在x轴定义线条的结束
* y2：在y轴定义线条的结束



## 四、文本

`<text>`该元素用来定义文本

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <text x="0" y="15" fill="red">I love SVG</text>
</svg>
```



### `<use>`标签

使用URI引用一个<G>,<svg>或其他具有一个唯一的ID属性和重复的图形元素。复制的是原始的元素，因此文件中的原始存在只是一个参考。原始影响到所有副本的任何改变。

标签自带的属性：

* x="克隆元素的左上角的x轴"
* y="克隆元素的左上角的y轴"
* width="克隆元素的宽度"
* height="克隆元素的高度"
* xlink:href="URI引用克隆元素"




## 五：stroke属性

stroke属性可以设置各种各种线条

一般我们拿他来设置线条的颜色