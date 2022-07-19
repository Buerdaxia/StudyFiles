# CSS的继承

继承可以让编码人员减少工作量。

关键字：`inherit`

使用`inherit;`关键字，可以让属性强制继承

以后写项目想要让不能继承的属性进行继承，就可以模仿下面的操作。

示例：

```css
/* 通用选择器，选择了所有标签，以及所有伪元素*/
*,
*::after,
*::before {
	margin: 0;
	padding: 0;
	box-sizing: inherit;/*1.表示所有标签的box-sizing属性都可以继承*/
}

/*2.注意：box-sizing属性默认是不会继承的*/
body {
  /*3.这样之后，所有body下的元素都会继承box-sizing属性*/
  box-sizing:border-box;
}
```









**注意：继承只会继承计算后的值，而不是继承声明**

示例：

```css
.parent {
  font-size: 20px;
  line-height: 150%; /*行高的计算方式 字体大小 * 百分比 */
}
/*line-height会继承下去，并且值时已经计算过的，line-height: 20px * 150% = 30px */
.child {
  font-size: 25px;
}
```





一般来说：字体相关的属性都会继承

