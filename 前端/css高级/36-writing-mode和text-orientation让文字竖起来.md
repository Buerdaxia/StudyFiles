# writing-mode和text-orientation让文字竖起来

**`writing-mode`** 属性定义了文本水平或垂直排布以及在块级元素中文本的行进方向。为整个文档设置该属性时，应在根元素上设置它（对于 HTML 文档，应该在 `html` 元素上设置）

**`text-orientation`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性设定行中字符的方向。但它仅影响纵向模式（当 [`writing-mode`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode) 的值不是`horizontal-tb`）下的文本。此属性在控制使用竖排文字的语言的显示上很有作用，也可以用来构建垂直的表格头。



这两个属性可以去mdn上看一下，讲的很详细。



## 实例：滚动数字

![19滚动数字](C:\Users\10854\Desktop\clone\StudyFiles\前端图片\css高级\19滚动数字.png)



核心思路：每一个都是个位数，所以我们可以让0~9竖直排开，然后当指定数字移动到指定的位置就可以了，

核心：数值和transform要进行绑定，公式需要自定义



4个核心属性：

1.writing-mode：vertical-lr

让0123456789数值排列

![image-20230317154445650](C:\Users\10854\Desktop\clone\StudyFiles\前端图片\css高级\19滚动数字-2.png)

2.text-orientation: upright

让所有数字的方向正确，

![19滚动数字-3](C:\Users\10854\Desktop\clone\StudyFiles\前端图片\css高级\19滚动数字-3.png)



3.通过transform来上下移动



4.通过transition属性来获得动画过渡效果，最后overflow：hidden隐藏掉多余的就可以了