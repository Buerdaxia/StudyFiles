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






## 五：stroke属性

stroke属性可以设置各种各种线条

一般我们拿他来设置线条的颜色





# `<use>`标签（究极重要）

使用URI引用一个<G>,<svg>或其他具有一个唯一的ID属性和重复的图形元素。复制的是原始的元素，因此文件中的原始存在只是一个参考。原始影响到所有副本的任何改变。

标签自带的属性：

* x="克隆元素的左上角的x轴"
* y="克隆元素的左上角的y轴"
* width="克隆元素的宽度"
* height="克隆元素的高度"
* xlink:href="URI引用克隆元素"



具体使用示例：

```html
<svg>
	<use xlink:href="img/sprite#icon-magnifying-glass"></use>
</svg>
```

注意：这里使用了精灵图版本的svg。

简单介绍一下：`xlink:href`它先要**跟上一个路径**，然后后面放上一个**哈希值**，#后面的是精灵图中每个图标的**`id`值**



## 修改`svg`的大小

`svg`图片修改大小真的简单的雅痞，直接修改它的`width`和`height`就行





## 修改`svg`的颜色

用一个比较特殊的`css`属性来进行修改：`fill`

可能是`svg`专属的，我没在其他地方用过这个属性。

示例：

```html
<svg>
	<use ..></use>
</svg>

<style>
  svg {
		fill: red; /*将svg图标的颜色修改为了红色*/
  }
  
  svg {
    color: blue; 
    fill: currentColor; /*这时currentColor的值就是blue*/
  }
</style>
```



注意：这个属性还有一种炫酷的简写方式，就是这个fill啊，还有个特殊值叫`currentColor`（这个值挺好用的，10分好用甚至9分好用）

还有一个注意点：`currentColor`是css的值，这意味着别的元素也能够用例如`border-bottom: 1px solid currentColor`

这个值的意思，**会自动获取当前元素的颜色，如果没有就获取父元素的颜色**





## svg图标下面多出来一丢丢的原因

使用svg图片时，会发现它会在下面多出来一点儿，其实主要原因就是，svg其实表现形式类似于`inline`，而只要是inline类的标签都会有这个通病。

解决方式：直接给`svg`设置一个`display:	flex`就能解决，或者`display:`block`





## 在css中使用svg

利用`css`的属性`background-image`也可以使用svg图像

示例：

```scss
// 在css中使用小图标，一般要么放在前面，要么放在后面，刚好！我们有before和after两个伪类，可以放图像
div::before {
  background-image: url(../img/chevron-thin-right.svg);
	background-size: cover;
}
// 但是这样子就会有一个问题，无法改变图像的颜色你知道吧。
```



但是如果使用一个非常新的属性`mask-image`来替换`background-image`是可以实现改变颜色的。

`mask-image`作用：用来设置元素上面的蒙版效果

核心：我们先设置一个背景颜色，然后让svg图像作为蒙版，只展示svg图像的位置，我们可以修改背景颜色，这样svg图标也会跟着改变颜色了

示例：

```scss
&__item::before {
  content: '';
  display: inline-block;
  height: 1rem;
  width: 1rem;
  margin-right: .7rem;

  // 老式浏览器使用background-image
  // background-image: url(../img/chevron-thin-right.svg);
  // background-size: cover;


  // 现代浏览器，使用掩码masks
  background-color: var(--color-primary);
  -webkit-mask-image: url(../img/chevron-thin-right.svg);
  -webkit-mask-size: cover;
  mask-image: url(../img/chevron-thin-right.svg);
  mask-size: cover;
}
```

