# uni中使用animate.css不生效问题

将`animate.css`中的:

```css
:root {
  --animate-duration: 1s;
  --animate-delay: 1s;
  --animate-repeat: 1;
}
```



修改为：

```css
page {
  --animate-duration: 1s;
  --animate-delay: 1s;
  --animate-repeat: 1;
}
```

原因：小程序，app中，根元素是page不是这个:root