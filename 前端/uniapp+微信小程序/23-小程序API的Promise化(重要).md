# 小程序API的Promise化(重要)

首先，小程序官方提供的异步API都是基于回调函数实现的，回调函数的缺点就不用多说了⑧。



API Promise化，就是通过额外配置，将官方的API基于异步的，升级改造为基于Promise的异步API。



## 实现API Promise化

在小程序中，实现API Promise化主要依赖于`miniprogram-api-promise`这个第三方包

安装：

```js
npm install -s miniprogram-api-promise@1.0.4
```



引入：

```js
//app.js
import {promisifyAll} from 'miniprogram-api-promise'
const wxp = wx.p = {};

// 会将wx上的apipromise化后，挂载到wxp上
promisifyAll(wx, wxp);
```



使用：

```js
async getInfo() {
  // 调用wx.p身上的request方法，会返回一个Promise对象
  const {data:res} = await wx.p.request({
    method: 'GET',
    url: 'http://applet-base-api-t.itheima.net/api/get',
    data: {
      name: 'zs',
      age: 20
    }
  })

  console.log(res)
},
```

