# nvue的一些小问题

## 一、css方面

1. 默认开启flex布局，并且只支持flex布局，默认`flex-direction: column`、`flex-wrap: nowrap`，注意**它默认direction方向和平时H5页面是反过来的**
2. 只有px单位，px单位的计算方式和小程序中的rpx和uni中的upx是一致的，都是讲屏幕分成750份
3. 没有百分比单位，例如100%，50%这些都是无效的，如果想要一半你就写375px，如果想要100%，那就是750px，高：1250px = 100%，宽：750px = 100%
4. 背景颜色只能写`background-color`
5. **只支持单类的选择器**，比较复杂的选择器就不行，例如`.box>item`这样的就不行，必须要给一个类名
6. 引入css只能通过`<style src="../../xx.css"></style>`方式
7. **任何week官方组件发现没有展示出来，记得自己加个宽高的样式，这样大概率能解决不显示的问题(●'◡'●)**

示例：

```vue
<template>...</template>
<script>xxx..</script>
<style src="../common/common.css"></style>
<style>
// 再编写自己的样式
</style>
```



css更加具体的限制请查看week官网：http://emas.weex.io/zh/docs/styles/common-styles.html#%E7%9B%92%E6%A8%A1%E5%9E%8B





## 二、上拉刷新实现

nvue中上拉刷新，建议使用`<loading>`标签来实现，该标签自带一个loading事件，就是监听list或者scroll等列表标签，到底部被上拉时触发。

示例：

```vue
<list>
...
  <!-- 上拉加载更多 -->
  <loading :display="loadingShow" @loading="onLoading">
        <text>loading</text>
        <loading-indicator></loading-indicator>
      </loading>
</list>
<script>
	export default {
    data() {
      loadingShow: 'hide'
    },
    methods: {
      // 上拉加载事件
      onLoading() {
          this.loadingShow = 'show';
          setTimeout(() => {
            // 追加数据
            this.lists = [...this.lists, ...['F', 'G']];
            this.loadingShow = 'hide';
          }, 2000)
      }
    }
  }
</script>
```



## 三、下拉加载更多的实现

首先我们要理解一下下拉加载更多，下拉一般会有一个限制效果，就是你下拉了多少px之后，才会进行刷新，一般我们都会给一个比较小的px值，来实现他下拉屏幕就给他加载的效果。



实现下拉加载更多更过`<refresh>`身上的两个自带的加载事件，第一个是`refresh`刷新事件、另一个`pullingdown`事件

示例：

```vue
<template>
	<list>
      <refresh @refresh="pullDownRefresh" @pullingdown="pulling" :display="show">
        <text>{{pullText}}</text>
      </refresh>
    ...
  </list>
</template>
<script>
  export default {
    data() {
      return {
        loadingShow: 'hide',
        show: 'hide',
        pullText: '下拉可以刷新',
        lists: ['A', 'B', 'C', 'D', 'E']
      }
    },
    methods: {
      // 上拉加载事件
      onLoading() {
          this.loadingShow = 'show';
          setTimeout(() => {
            // 追加数据
            this.lists = [...this.lists, ...['F', 'G']];
            this.loadingShow = 'hide';
          }, 2000)
      },
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
        if(e.pullingDistance > e.viewHeight) {
          this.pullText = '松开手指下拉数据';
        }
      }
    }
  }
</script>
```



## 四、nvue与vue页面的通讯

使用uni对象身上自带的方法：`uni.$emit`和`uni.$on`即可



## 五、nvue和vue页面的共享的变量

首先：nvue是不支持vuex的。

**共享变量的解决方式一**：

使用本地存储uni.storage来存储变量，vue和nvue的界面都可以使用uni.storage来存储，并且存储是持久化的。



**共享变量的解决方式二**：(这个不是实时的)

globalData，小程序中有一个globalData机制，这套机制同样适用于uni中并且**全端通用**，在App.vue中定义globalData，通过getApp().globalData来获取





## 六、nvue没有onShow生命周期的替代方式

由于nvue中是没有`onShow`这个生命周期的，但是如果非要用这个钩子（我觉得应该没有必要），我们可以在created生命周期中，写入以下代码：

注意：是nvue文件

```js
  const currentWebview = plus.webview.currentWebview()
  ...
created() {
  // 监听当前窗口显示
  currentWebview.addEventListener('show', e => {
    console.log('indexShow');
  })
},
beforeDestroy() {
  // 销毁前移除监听
  currentWebview.removeEventListener('show', e=> {});
},
```

这样，一进入nvue页面，就会调用后面那个回调，输出`indexShow`，在页面销毁前，为了性能，把他移除掉就行了



## 七、list组件的小问题

list组件是垂直列表组件，他外面不能包裹div，如果又包裹标签，那么会失去垂直滚动的能力。