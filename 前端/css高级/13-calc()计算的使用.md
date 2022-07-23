#  calc()计算的使用

calc()计算函数在一些特定的场合就非常实用。它可以帮助我们返回一个计算值。

语法：

```css
/*100%依赖于当前选择器的容器*/
calc(100% - 20px);
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



