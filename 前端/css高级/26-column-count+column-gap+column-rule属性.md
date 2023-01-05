# column-count+column-gap+column-rule属性

这三个属性可以让一段文字进行分栏操作。



1. column-count：让一段文字分成几栏
2. column-gap：分栏之间的距离
3. column-rule：分栏之间的隔断样式



示例：

```scss
&__text {
  font-size: 1.4rem;
  margin-bottom: 4rem;

  // 文本的三个column属性
  column-count: 2; // 2列
  column-gap: 4rem; // 2列之间的差距
  column-rule: 1px solid $color-gray-light-2; // 两列中间的竖线

  // 这个是英文换行时的连字符
  hyphens: auto;
}
```

