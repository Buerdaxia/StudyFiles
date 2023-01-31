# 全屏 screenFull插件使用

可以直接去`npm`官网搜索`screenFull`有详细的文档

>安装

```js
npm install screenfull@5.1.0
// 最新版可能有问题
```



这里直接贴screenFull组件了

>前提:需要svg-icon组件，和两张svg图，一张'exit-fullscreen.svg'一张'fullscreen.svg'



创建`screenFull.vue`并写入

```vue
<template>
  <div @click="handleFullScreen">
    <svg-icon
      :icon="icon === true ? 'exit-fullscreen' : 'fullscreen'"
    ></svg-icon>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import screenfull from 'screenfull'

const icon = ref(screenfull.isFullscreen)
const handleFullScreen = () => {
  if (screenfull.isEnabled) {
    screenfull.request()
  }
}

const changeIcon = () => {
  icon.value = screenfull.isFullscreen
}

// 解决响应式失效问题
onMounted(() => {
  screenfull.on('change', changeIcon)
})

onBeforeUnmount(() => {
  screenfull.off('change', changeIcon)
})
</script>

<style lang="scss" scoped></style>

```

## 小问题

ref并不能监听`screenfull.isFullscreen`的变化，官方也给了解决方式，就是利用` screenfull.on`来监听变化。所以上面的两个声明周期钩子和一个`changeIcon`函数就是用来解决点击图片无反应的。