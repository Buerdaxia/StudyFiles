#  CSS的position的五种属性

## 1：position：relative;相对定位

1）不影响元素本身特性（无论区块元素还是内联元素会保留其原本特性）

2） 不会使元素脱离文档流（元素原本位置会被保留，即改变位置也不会占用新位置）

3） 没有定位偏移量时对元素无影响（相对于自身原本位置进行偏移）

4）提升层级（用z-index样式的值可以改变一个定位元素的层级关系，从而改变元素的覆盖关系，值越大越在上面，z-index只能在position属性值为relative或absolute或fixed的元素上有效。） （两个都为定位元素，后面的会覆盖前面的定位）

**z-index必须要设置一下position才能设置**

## 注意点

**记然你使用position，就一定要配合使用top，right，bottom，left这四个属性，来控制元素的位置**

记住没：再说一遍，只要position了，就一定要使用top、right、bottom、left这四个属性，

重要的事情说三遍：只要使用position了，就一定要使用top、right、bottom、left！！！



## 2：position：absolute；绝对定位

1）使元素完全脱离文档流（在文档流中不再占位）

2）使行内级元素可以设置宽高的时候支持宽高（改变了内联元素的特性）

3）相对于最近一个有定位的父元素偏移（若其父元素没有定位则逐层上找，直到document——页面文档对象）

4）相对定位一般配合绝对定位使用（子绝父相）

5）提升层级



## 3：position：fixed；固定定位

1）会脱标

fixed生成的固定定位的元素，相对于浏览器窗口进行定位。



经常和一个布局等式一起使用

​	**定位参照对象的宽度=left+ margin-left +right+ margin-right +内容实际宽度**

​	**定位参照对象的高度=top+ margin-top +bottom+ margin-bottom +内容实际高度**



## 4：position：static；默认值

默认布局。元素出现在正常流中。

## 5：position：sticky；粘性定位

1）会脱标

粘性定位，该定位基于用户滚动的位置。

它的行为很像position：relative但是当页面滚动超出目标区域时，它的表现就和position:fixed一样会固定再目标位置。

**元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。**

**这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同**

**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix 。



注意2：**这个属性，如果不生效，可以尝试在元素外层再套上一个div，这样就会生效（我也不知道为啥）**



