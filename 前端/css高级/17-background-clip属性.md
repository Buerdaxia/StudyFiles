# background-clip属性

注意：这是一个非常新的属性，新到使用时要加上-webkit-

作用：

`background-clip` 设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

**还有一个特殊的作用，就是当值时`text`时，会把元素背景映射到文字的位置上。**



示例：利用该属性，制作渐变字体

```scss
$color-primary-light: #55c57a;
$color-primary-dark: #28b485;
.heading-secondary {
  // 注意：这里最好也设置成inline-block让字体撑开元素
  display: inline-block;
  font-size: 3.5rem;
  // 1.先设置一个渐变的背景颜色
  background-image: linear-gradient(to right, $color-primary-light, $color-primary-dark);
  // 2.使用clip属性，让背景只显示到文字的位置上
  -webkit-background-clip: text;
  // 3.让字体透明
  color: transparent;
  letter-spacing: 2px;
  transition: all 0.2s;
}

// 这样3步设置下来，就能够得到一个有渐变特效的字体了
```



剩下的用法，请查MDN
