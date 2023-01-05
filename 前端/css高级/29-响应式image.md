# 响应式image

## 响应式图片的三种情况

### 情况一：排列的不同

第一种情况是不同屏幕，图片的排列方式不同，例如大屏我们横向排列，小屏幕就要竖向排列。



解决方式：

我们可以在css中书写@media查询的宽度版本来进行控制。



### 情况二：分辨率不同

不同屏幕所展示的分辨率不同，例如一些2k屏幕就需要更高像素的图片，普通的1080p就不需要

简而言之：**大屏幕要提高更高质量的图片，小屏幕提供低一些的即可**



解决方式：

一：利用img标签自带的srcset属性来进行设置。

示例：

```html
<img srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x" alt="完整图标">

<!-- 浏览器会自动判断屏幕的分辨率，选择图片，如果高分辨率就2x会选择第二张图片的路径，如果低分辨率就是1x会选择第一个图片的路径-->
```



二：利用@media查询的分辨率版本来进行控制

示例：

```scss
.header {
	position: relative;
	/* 视口的95% */
	height: 95vh;
	/* 设置2个背景图片:1线性渐变，2hero图片 */
	/* linear-gradient线性渐变,参数一渐变方向,剩下参数为渐变颜色 */
	background-image: linear-gradient(
			to right bottom,
			rgba($color-primary-light, 0.8),
			rgba($color-primary-dark, 0.8)
		),
		url(../img/hero-small.jpg); // small的分辨率低一些

  @media (min-resolution: 192dpi) { // 每英寸点数大于192时
    // 使用高分辨率图片
    background-image: linear-gradient(
			to right bottom,
			rgba($color-secondary-light, 0.8),
			rgba($color-secondary-dark, 0.8)
		),
		url(../img/hero.jpg);
  }
}
```



### 情况三：不同图片

这种情况是，不同设备之间，展示的图片不相同。

解决方式：

一：利用img标签的srcset属性 + sizes属性



二：利用picture标签 + source标签 + img标签来解决



这两个方式都在，外面响应式布局那个文件夹中`../响应式布局`