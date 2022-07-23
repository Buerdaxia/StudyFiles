# Emoji的使用

`Emoji`表情怎么在标签中使用呢？

通过:

```html
<!-- aria-label="smirk"是为残障人士们做的优化，当读过这条代码时，会读label中的内容 -->
<span role="img" aria-label="smirk">😂</span>
```

这样就会被表情就会被当作一个图片渲染，并且有一个`label`

