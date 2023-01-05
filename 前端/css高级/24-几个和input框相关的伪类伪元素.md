# 几个和input相关的伪类伪元素

这里写几个超级好用的伪类和伪元素，用这几个伪类伪元素可以实现超级炫酷无敌帅的input框

## :focus伪类

当input框选中时，会触发该伪类。

我们可以利用这个伪类，在`input`框被选中时，给输入框添加一些独特的样式让我们的输入框效果更好。

示例：

```scss
// 选中时隐藏所有外边框，添加一个底边框
input:focus {
  outline: none;
  box-shadow: 0 1rem 2rem rgba($color-black, .1);
  // 防止添加边框时，底部变大发生抖动，我们可以给一个透明的默认边框先把位置占着
  border-bottom: 3px solid transparent;
  
  // 选中时边框颜色
  border-bottom: 3px solid $color-primary;
}
```



## :invalid伪类

无效伪类，需要结合input自带的一些表单验证来使用，当表单验证不通过时会触发该伪类的效果

例如`<input ... required />`这样的就是必填的`input`框，如果没有填写内容，就会触发`:invalid`伪类

示例：

```scss
&:focus:invalid {
  border-bottom: 3px solid $color-secondary-dark;
}
```





## :placeholder-shown伪类

这个伪类更牛逼，是当placeholder出现时，触发该伪类。

我们可以配合一些`<label>`标签，结合这个伪类，来做一些当placeholder消失时的效果，展示时的效果

示例：



```scss
// + 相邻兄弟选择器， ~跨越兄弟选择器
input:placeholder-shown + label {
  // 这个是placeholder展示时，label的样式
  opacity: 0;
  visibility: hidden;
  // 展示时，先让他和placeholder重叠，然后等placeholder消失时就会重置位置
  transform: translateY(-4rem);
}
```



## ::placeholder伪元素

这个伪元素就是选中`placeholder`的，可以给它添加样式。

示例：

```scss
// 让placeholder是红色（虽然有点丑）
input::placeholder {
  color: red;
}
```







## :checked伪元素（单选，多选框）

这个伪元素，是当input是单选框或者多选框时使用的，使用这个伪元素表示当input被选中时的选择器

常常用来自定义单选或者多选框