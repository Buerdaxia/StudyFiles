# 触发事件`transitionend`

详情请查询：`mdn`(●'◡'●)

`transitionend`事件是类似于`click`这样的可绑定事件。

触发：在`transition`结束后就会触发。

注意：`transitionend`事件会多次触发，每一个会变化的属性都会被监听并且触发该事件。

示例：

```js
const panels = Array.from(document.querySelectorAll('.panel'));
panels.forEach((panel) => {
  panel.addEventListener('click', clickHandler);

  // transitioned会多次触发！！
  panel.addEventListener('transitionend', transitionHandler);
})
```

