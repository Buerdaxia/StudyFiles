# vue项目自定义SVG组件

webpack下使用svg首先安装`loader`

```
npm i --save-dev svg-sprite-loader@6.0.9
```



## 一、创建svg组件

>在`components`下新键`SvgIcon/index.vue`

```vue
<template>
  <svg class="svg-icon" aria-hidden="true">
    <!--这个是svg的use链接别的svg -->
    <use :xlink:href="iconName"></use>
  </svg>
</template>

<script setup>
// defineProps就是之前的props
import { defineProps, computed } from 'vue'
const props = defineProps({
  icon: {
    type: String,
    required: true
  }
})
// 并不知直接使用 需要加一个#
const iconName = computed(() => {
  return `#icon-${props.icon}`
})
</script>

<style lang="scss" scoped>
// 这是svg图片的通用css设置  
.svg-icon {
  // em基于当前父元素的font-size，
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>


```

### 讲解fill：currentColor属性

这个是svg的一个属性，表示填充颜色，设置成`fill: currentColor;`表示如果当前css元素没有设置color，则会继承父元素的`color`

注意：**如果要使这个元素生效，还有一点要删除svg文件中的所有fill:xxx属性，有两个要删除**



## 二、配置webpack

> 1.创建`src/icons/index.js`
>
> 注意：icons下是放置我们自己的svg图片的

```js
import SvgIcon from '../components/SvgIcon/index.vue'
// 这一段是webpack的配置
const svgRequired = require.context('./svg', false, /\.svg$/)
svgRequired.keys().forEach(item => {
  svgRequired(item)
})
// 注意这里，不同版本注册组件方式不同，vue2是Vue.component()
export default app => {
  app.component('svg-icon', SvgIcon)
}

```

讲解一下：**require.context**

函数 = require.context(参数一，参数二，参数三)接受三个参数

1. directory {String} -读取文件的路径
2. useSubdirectories {Boolean} -是否遍历文件的子目录
3. regExp {RegExp} -匹配文件的正则



例如，webpack官网例子：

```js
require.context('./test', false, /.test.js$/);
// 匹配test文件夹下所有以.test.js结尾的文件，不遍历子文件夹
```

在index.js中调用 **require.context('./test', false, /.test.js$/);**会得到test文件下3个文件的执行环境

值得注意的是require.context函数执行后返回的是**一个函数,并且这个函数有3个属性**

1. resolve {Function} -接受一个参数request,request为test文件夹下面匹配文件的相对路径,返回这个匹配文件相对于整个工程的相对路径
2. keys {Function} -返回匹配成功模块的名字组成的数组
3. id {String} -执行环境的id,返回的是一个字符串,主要用在module.hot.accept,应该是热加载?





> 2.在`main.js`中引入

```js
// 暴露的是一个函数
import SvgIcon from './icons'

const app = createApp(App)
SvgIcon(app)
```



> 3.修改webpack配置，修改`vue.config.js`

```js
// element-plus配置
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')
// svg配置引入
const path = require('path')
function resolve(dir) {
  return path.join(__dirname, dir)
}
const webpack = require('webpack')
module.exports = {
  // element-plus配置
  configureWebpack: config => {
    config.plugins.push(
      AutoImport({
        resolvers: [ElementPlusResolver()]
      })
    ),
      config.plugins.push(
        Components({
          resolvers: [ElementPlusResolver()]
        })
      )
  },
  
  // svg配置
  chainWebpack(config) {
    // 设置 svg-sprite-loader
    // config 为 webpack 配置对象
    // config.module 表示创建一个具名规则，以后用来修改规则
    config.module
      // 规则
      .rule('svg')
      // 忽略
      .exclude.add(resolve('src/icons'))
      // 结束
      .end()
    // config.module 表示创建一个具名规则，以后用来修改规则
    config.module
      // 规则
      .rule('icons')
      // 正则，解析 .svg 格式文件
      .test(/\.svg$/)
      // 解析的文件
      .include.add(resolve('src/icons'))
      // 结束
      .end()
      // 新增了一个解析的loader
      .use('svg-sprite-loader')
      // 具体的loader
      .loader('svg-sprite-loader')
      // loader 的配置
      .options({
        symbolId: 'icon-[name]'
      })
      // 结束
      .end()
    config
      .plugin('ignore')
      .use(new webpack.ContextReplacementPlugin(/moment[/\\]locale$/, /zh-cn$/))
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
  }
}

```



## 三、使用

>配置完毕，放好要使用的svg，之后直接使用svg-icon组件即可

```vue
<svg-icon icon="user" class="svg-container"></svg-icon>
```

注意：user是svg的文件名，在`icons`文件夹下有`user.svg`

