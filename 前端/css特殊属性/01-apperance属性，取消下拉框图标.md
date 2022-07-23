# apperance属性，取消下拉框图标

该属性可以删除下拉框复选框等等的图标，但是有浏览器要求。

`chrome`下：

```css
--webkit-appearance: none;
```

`firefox`下：

```css
--moz-appearance: none;
```



`ie`下：

```css
::-ms-expand {
  display: none;
}
```



react脚手架creat-react-app包含了`Autoprefixer`可以直接写：

```css
appearance: none;
```

