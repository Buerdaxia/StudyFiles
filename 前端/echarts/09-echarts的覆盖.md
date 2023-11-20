# echarts的覆盖

场景：

有时候我们想要根据我们的选项实时的去控制echarts中展示的数据，这时候我们有需要配置一下`setOption`方法的第二个参数





## 具体操作：

```js
chart.setOptons(option, true)
```

没错，给第二个参数传递true即可，官网上有详细介绍，简单来说就是，**第二个参数如果为 `true`，表示所有组件都会被删除，然后根据新 `option` 创建所有新组件。**