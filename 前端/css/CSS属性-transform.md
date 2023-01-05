# CSS属性-transform

css transform属性允许你旋转，缩放，倾斜或者平移给定元素

常见的函数transform function：

* translate(x,y)：平移
* scale(x,y)：缩放
* rotate(deg)：旋转
* skew(deg,deg)：倾斜

**平移translate(x,y)函数：**

* 值个数：
  * 一个值x，设置x轴上的位移
  * 两个值x，y，设置x轴和y轴上的位移

* 值类型：
  * 数字：100px
  * 百分比：擦按照元素本身

**缩放scale(x,y)函数：**

* 值个数：
  * 一个值x，设置x轴和y轴相同的缩放
  * 二个值x，y，设置x轴的缩放和y轴上的缩放

* 值类型：
  * 数字：1：保持不变 2：放大一倍 0.5：缩小1倍
  * 不支持百分比



**旋转rotate(deg)函数**：rotate（45deg）；

* 值个数：
  * 一个值表示旋转角度

* 值类型：
  * deg：旋转角度
  * 正数为顺时针
  * 负数为你是在

**旋转角度受到transform-orgin影响**



**倾斜skew(x,y)函数：**

* 值个数：

  * 一个值时，表示x轴上的倾斜
  * 两个值时，表示x轴和y轴上的倾斜

* 值类型：

  * deg：倾斜角度

    

## CSS属性transform-origin

transform-origin：设置变形的原点

* 一个值：设置x轴的原点
* 两个值：设置x轴和y轴的原点
* 必须是<length>,<percentage>,或者left，center，right，top，bottom关键字中的一个
  * length：从左上角开始计算
  * 百分比：参考元素本身大小



## transform属性的注意点

### 添加多个transform属性

假如，现在你想给一个选择器，即设置它能够向左移动，同时还要倾斜15deg，这时候该怎么做呢？

**先看错误示范：**

```css
.el {
  transform: translateX(-3rem);
  transform: skewX(15deg);
}
/*
	注意你要是这样写，那就有问题了，css有一个规则，就是同名的属性，下面的会覆盖掉上面的。
这样写最终只会让skewX生效
*/
```



**正确例子：**

```css
.el {
  transform: translateX(-3rem) skewX(15deg);
}
/*
transform支持同时设置多个变换呦(●'◡'●)
*/
```





### 行内元素，无法使用translate

注意：**当`display: inline`的元素都不能进行`translate`**

所以说：如果你使用`transform: translate(...)`时，发现元素无法移动，建议检查一下标签的**display值**
