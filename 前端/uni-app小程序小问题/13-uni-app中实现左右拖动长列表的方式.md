# uni-app中实现左右拖动长列表的方式

这里是使用两个内置组件：

通过`swiper`+`scroll-view`这两个组件来实现滚动长列表方案。

水平的滚动通过`swiper`来实现，`swiper-item`的垂直方向滚动，通过scroll-view来实现。

有个问题：scroll-view无法实现下拉刷新。（注意：这里指的是swiper内部的那个垂直方向的scroll-view）





第二种方式：`nvue`

通过slider+list来实现的

使用week的在list组件，list组件中的就解决了scroll-view无法下拉刷新的问题

下拉示例：

```vue
<template>
    <list>
      <refresh @refresh="pullDownRefresh" @pullingdown="pulling" :display="show">
        <text>{{pullText}}</text>
      </refresh>
      <cell>
        <text style="width: 100%; height: 500px; border: 1px solid #000; margin: 20px;" >内容罢了</text>
      </cell>
      <header>
        <text style="width: 100%; height: 40px; background-color: antiquewhite;">我是顶部</text>
      </header>
      <cell v-for="(num, index) in lists" :key="index">
        <text style="width: 100%; height: 500px; border: 1px solid #000; margin: 20px;" >{{num}}</text>
      </cell>
    </list>
</template>

<script>
  export default {
    data() {
      return {
        show: 'hide',
        pullText: '下拉可以刷新',
        lists: ['A', 'B', 'C', 'D', 'E']
      }
    },
    methods: {
      // 执行刷新
      pullDownRefresh(e) {
        this.show = 'show';
        this.pullText = '正在刷新中';
        setTimeout(() =>  {
          this.pullText = '下拉可以刷新';
          this.show = 'hide'
        }, 2000)
      },
      // 下拉就触发，会获取一系列数据
      pulling(e) {
        // 下拉距离，大于refresh标签的距离时
        if(e.pullingDistance > e.viewHeight) {
          this.pullText = '松开手指下拉数据';
        }
      }
    }
  }
</script>

<style>
</style>
```

