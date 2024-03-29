# object-fit和object-position



## objcet-fit

功能：**`object-fit`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性指定[可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)的内容应该如何适应到其使用的高度和宽度确定的框。

具体请看MDN

注意：**你要使用这个属性，你必须要手动给图片设置宽高，否则不会生效**

示例：

```scss
img {
  width: 100%;
  height: 100%;
  object-fit: cover; // 这样设置才会有效果
}
```





## object-position

功能：[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性 **`object-position`** 规定了[可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)的内容，在这里我们称其为对象（即 **`object-position`** 中的 **`object`）**，在其内容框中的位置。可替换元素的内容框中未被对象所覆盖的部分，则会显示该元素的背景（[`background`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background)）。

具体请看MDN