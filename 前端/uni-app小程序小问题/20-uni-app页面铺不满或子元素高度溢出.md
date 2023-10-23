# uni-app设置页面高度铺满，子元素自适应



方法：使用flex弹性布局

1，父元素（容器）指定为flex弹性布局，主轴方向flex-direction为column，宽度width为100%，高度height为100vh。

2，固定高度的子元素，width: 100%;height: 100rpx;

3，需要铺满的子元素，width: 100%;flex: 1;

在`APP.vue`中：

```scss
page {
  display: flex;
  flex-direction: column;
  height: 100vh;
  // 关键是上面

  background-color: #F6F7FB;
  font-size: 12px;
}
```



在其他页面中：

```vue
<template>
	<view class="bg"></view>
</template>
...

<style lang="scss" scoped>
.bg {
  box-sizing: border-box;
  flex: 1;
  width: 100%;
  background: #fff;
}
</style>
```

