# vue-seamless-scroll无缝滚动插件

官网：https://chenxuan0000.github.io/vue-seamless-scroll/zh/guide/usage.html#%E4%BD%BF%E7%94%A8%E7%BB%84%E4%BB%B6



最基本使用方式：

```vue
<template>
  <vue-seamless-scroll :data="listData" class="warp">
    <ul class="item">
      <li v-for="(item, index) in listData" :key="index">
        <span class="title" v-text="item.title"></span>
        <span class="date" v-text="item.date"></span>
      </li>
    </ul>
  </vue-seamless-scroll>
</template>

<script>
  import vueSeamlessScroll from 'vue-seamless-scroll'

  export default {
    name: 'Example01Basic',
    components: {
      vueSeamlessScroll
    },
    data () {
      return {
        listData: [{
          'title': '无缝滚动第一行无缝滚动第一行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第二行无缝滚动第二行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第三行无缝滚动第三行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第四行无缝滚动第四行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第五行无缝滚动第五行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第六行无缝滚动第六行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第七行无缝滚动第七行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第八行无缝滚动第八行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第九行无缝滚动第九行',
          'date': '2017-12-16'
        }],
      }
    },
  }
</script>

<style lang="scss" scoped>
  .warp {
    height: 270px;
    width: 360px;
    margin: 0 auto;
    overflow: hidden;
    ul {
      list-style: none;
      padding: 0;
      margin: 0 auto;
      li,
      a {
        display: block;
        height: 30px;
        line-height: 30px;
        display: flex;
        justify-content: space-between;
        font-size: 15px;
      }
    }
  }
</style>
```





## 遇到的小坑

## 一：为什么页面中显示的数据数量不对，被overflow:hidden盖住的元素不展示（垂直滚动时）

原因：在`<vue-seamless-scroll></vue-seamless-scroll>`标签上的`class`类是不能设置`flex`布局的，当然垂直滚动时，基本上是不需要设置flex布局的。



示例：

```vue
<template>
  <vue-seamless-scroll :data="listData" class="warp">
    <ul class="item">
      <li v-for="(item, index) in listData" :key="index">
        <span class="title" v-text="item.title"></span>
        <span class="date" v-text="item.date"></span>
      </li>
    </ul>
  </vue-seamless-scroll>
</template>
<style lang="scss" scoped>
  .warp {
    /*display: flex;
    flex-diraction: column;*/
    /*注意：这里不能设置flex布局，设置后会导致页面数据被盖住*/
    height: 270px;
    width: 360px;
    margin: 0 auto;
    overflow: hidden;
    ul {
      list-style: none;
      padding: 0;
      margin: 0 auto;
      li,
      a {
        display: block;
        height: 30px;
        line-height: 30px;
        display: flex;
        justify-content: space-between;
        font-size: 15px;
      }
    }
  }
</style>
```

