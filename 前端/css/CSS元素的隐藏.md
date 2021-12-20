# CSS元素的隐藏

方式一：

display：none

**特点：元素隐藏，并且不再占据位置**

方式二：

修改高度为0 + overflow:hidden;

配合transition属性可以可以实现动画效果

```css
box1{
	overflow:hidden;
	height:0px;
	width:100%;
	transition: .5s;
}

a:hover box1{
	height: 140px;
}
//当a选中时，box1会慢慢下拉下来
```





方式三：

visibility:hidden;

**特点：元素虽然隐藏了，但是还是会继续占据原有的位置**

