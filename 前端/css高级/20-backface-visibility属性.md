# backface-visibility属性

实验性功能：指当元素的背面朝向观察者时，是否可见。

具体请去MDN查看。

语法：

```css
backface-visibility: hidden; /*隐藏，不可见*/

backface-visibility: visible; // 默认可见
```



目前用处：我目前用到它的位置，就是在例如**卡片旋转时**，正面卡片转到背面了，我把它隐藏掉



用处2：解决transform: translate动画抖动的问题

就是有时候，会发现，设置了一些移动动画，动画结束时会莫名其妙的抖动一小下，我们就可以用这个属性

```css
backface-visibility: hidden;
// 直接给父元素设置一下
```

