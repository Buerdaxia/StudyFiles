# 引导页driver.js的使用

加入公司一个项目为了优化用户体验，想要在界面上引导用户进行操作，例如：

![引导页图片](C:\Users\10854\Desktop\clone\StudyFiles\前端图片\driver.js\引导页图片.PNG)

可以使用`driver.js`

>第一步，安装

```
npm install driver.js
```



>第二步，编写引导步骤的代码，创建steps.js

```js
export const steps = t => {
  return [
    {
      // 第一个是元素，还是id选择器，所以会选中html中id为guide的元素
      element: '#guide',
      popover: {
        title: t('driver.guideBtn'),
        description: 'Body of the popover',
        position: 'left'
      }
    },
    {
      element: '#hamburger',
      popover: {
        title: t('driver.hamburgerBtn'),
        description: 'Body of the popover',
        position: 'bottom'
      }
    },
    {
      element: '#screenFull',
      popover: {
        title: t('driver.fullScreen'),
        description: 'Body of the popover',
        position: 'left'
      }
    }
  ]
}

```





>第三步，建立一个按钮组件，点击就会有引导出现

```vue
<template>
  <div id="guide" @click.stop="handleGuide">
    <svg-icon icon="guide"></svg-icon>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import Driver from 'driver.js'
import 'driver.js/dist/driver.min.css'
import { steps } from './steps'
import { watchLang } from '@/i18n/watchlang'
import i18n from '@/i18n'
const t = i18n.global.t
// 这个t等同于8.0版本的$t
let driver
// 2.在页面渲染完毕时调用
onMounted(() => {
  initDriver()
})
// 1.建立一个初始化配置对象函数
const initDriver = () => {
  driver = new Driver({
    animate: false, // Whether to animate or not
    opacity: 0.75, // Background opacity (0 means only popovers and without overlay)
    padding: 10, // Distance of element from around the edges
    allowClose: true, // Whether the click on overlay should close or not
    overlayClickNext: false, // Whether the click on overlay should move next
    doneBtnText: t('driver.doneBtnText'), // Text on the final button
    closeBtnText: t('driver.closeBtnText'), // Text on the close button for this step
    stageBackground: '#ffffff', // Background color for the staged behind highlighted element
    nextBtnText: t('driver.nextBtnText'), // Next button text for this step
    prevBtnText: t('driver.prevBtnText') // Previous button text for this step
  })
}

watchLang(initDriver)

// 3.编写点击函数
const handleGuide = () => {
  console.log(i18n)
  // 1.定义异步步骤
  driver.defineSteps(steps(t))
  // 2.启动
  driver.start()
}
</script>

<style lang="scss" scoped></style>

```

> 国际化的`watchlang.js`文件

```js
import { watch } from 'vue'
// 我们在vuex的getters中存了一份语言，所以可以监听语言lang是否修改
import store from '@/store'

export const watchLang = (...cbs) => {
  watch(
    () => store.getters.lang,
    () => {
      cbs.forEach(cb => {
        console.log('我是cb', cb)
        cb()
      })
    },
    { deep: true }
  )
}

```



## 国际化的小问题

如果要使用国际化，注意，上面的`initDriver`函数，只会在页面dom挂载完毕后加载一次，也就是只会`new`一次，而我们点击国际化切换后，语言虽然切换了，但是会造成**引导页的页面中英文并未切换，原因就在这里，因为只调用了一次**，所以我们在上面代码中，分装了一个`watchlang`文件，用来`watch`语言是否改变，如果改变就重新调用一次函数让`driver.js`的配置重新加载