# attr函数

attr函数只能使用在content属性中，该函数可以传入一个data-set系列的值作为参数，然后可以传递给content属性。

这个函数唯一好用的就是，它可以简化徽标的写法，我目前也只用它干这个

示例：

```css
.box::after {
  content: attr(data-count)
}
```



**小案例，用attr函数做一个右上角徽标：**

```vue
<template>
	<div>
    <!--data-count值的变化会比attr函数捕捉到-->
		<div class="box" data-count="0"></div>
	</div>
</template>
<style scoped>
.box {
	width: 100px;
	height: 100px;
	background-color: #bfc;
	position: relative;
}

.box::after {
	/* 这里使用attr函数获取data-count的值 */
	content: attr(data-count);
	text-align: center;
	color: white;
	line-height: 20px;
	position: absolute;
	right: 0;
	top: 0;
	width: 20px;
	height: 20px;
	border-radius: 50%;
	background-color: red;
}

/* 判断如果data-count的值是0，那么就将徽标隐藏 */
.box[data-count='0']::after {
	display: none;
}
/*当然这里也可以根据值来进行判断然后进行css书写*/
</style>
```

