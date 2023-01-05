# 04-v-loading的小问题

>改正：2022-11-22
>
>**element-ui的loading不会有这个问题，这个问题只会出现在我们自己封装的loading组件上**



首先将一下v-loading，它是element-ui的加载动画的设置，有两种方式，一种是通过api的方式调用，进行全局的加载切换(加载/不加载)，另一种是通过属性的方式来设置，设置到element组件的属性上`v-loading="true"`时为加载，v-loading="false"为不加载



## 渲染加载卡顿问题

一般我都是在这种场景进行使用，就是在请求数据时，先让页面加载，当数据请求完毕后，立马结束加载，具体如下：



示例：

```js
export default {
	data() {
    loading: false,
    dataForm: {}
  },
  
  methods: {
    getData() {
      // 1.加载动画启动
      this.loading = true;
      this.$http.get('...').then(({data:res}) => {
        // 2.数据请求完毕，关闭加载动画
        this.loading = false;
      }).catch((e) => {
        // 3.请求接口出错，也关闭
        this.loading = false;
      })
    }
  }
}
```



上面这种情况，在数据量不是很大的情况下，渲染速度足够快时，是不会有卡顿的，就是数据一请求完毕，关闭加载的同时，页面也渲染结束。



但是有时候数据量，过大，渲染的很慢，就是数据虽然已经请求回来，加载也关闭了，但是任然在进行渲染，**这时候就会出现加载已经关闭，但是数据还没渲染完的情况**。



### 解决方式

我们可以使用一个api，`this.$nextTick(() => {})`来关闭我们的加载动画。

这个api的作用是，在dom渲染结束后，再执行回调，所以我们可以利用这个机制，来实现让dom渲染结束后，再关闭加载动画，就不会出现上面的问题了。



示例：

```js
export default {
	data() {
    loading: false,
    dataForm: {}
  },
  
  methods: {
    getData() {
      // 1.加载动画启动
      this.loading = true;
      this.$http.get('...').then(({data:res}) => {
        // 2.数据请求完毕，关闭加载动画
        this.$nextTick(()=>{
          this.loading = false;
        })
      }).catch((e) => {
        // 3.请求接口出错，也关闭
        this.$nextTick(()=>{
          this.loading = false;
        })
      })
    }
  }
}
```



## 解释：

奥~，好像原生的v-loading源码中就用了这个来解决了，我nt了。不过上面这个可以解决我们自己写的loading动画问题。