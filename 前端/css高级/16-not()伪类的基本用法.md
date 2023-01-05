# :not()伪类的基本用法

语法：

```css
选择器:not(伪类选择器(选择器)) {
  // 除了括号里的选择器，都会添加的元素
}
```



示例：

```scss
.row {
  // 最大宽度：如果能放下最大宽度，则是114rem，如果放不下，直接占用100%
  max-width: $grid-width;
  background-color: #eee;
  margin: 0 auto;

  
  // 除了最后一个.row都会添加一个margin-bottom属性
  &:not(:last-child) {
    margin-bottom: $gutter-vertical;
  }
}
```



示例2：

**下面这个更是重量级**

```scss
// composition选中时，选择她下面composition__photo没有被选中的，
.composition:hover composition__photo:not(:hover) {
	transform: scale(0.9);
}
```

