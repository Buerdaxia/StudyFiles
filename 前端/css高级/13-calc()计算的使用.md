#  calc()计算的使用

calc()计算函数在一些特定的场合就非常实用。它可以帮助我们返回一个计算值。

注意：**这个函数最强大的就体现在，它可以进行混合单位的运算，你知道这是什么概念嘛，100%可以和rem，px进行运算**

语法：

```css
/*100%依赖于当前选择器的容器*/
calc(100% - 20px); /*注意，运算符两边一定要有空格，否则回有问题*/
```



例如：

```css
<div class='container'>
	<div class='content'>我是内容<div>
	<button>我是按钮</button>
</div>

/*我们像设置content绝对定位，并且在按钮上方12px处*/
.container {
  position: relative
}

.content {
  position: absolute;
  
  bottom: calc(100% +12px);
}
```



## sass中使用calc函数

当在sass中使用calc并且使用了sass的变量时，必须要这样写：

示例：

```scss
// 必须在变量前用#{}包裹一下
$gutter-horizontal: 8rem;
width: calc((100% - #{$gutter-horizontal}) / 2);
```



