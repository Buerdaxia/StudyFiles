# uni-app配置网络请求

由于uni自带的请求方式`uni.request`功能有一点点儿少，我们可以自己分装。

或者用老师的库`@escook/request-miniprogram`，直接去npm搜就行了。



安装：

```
npm install @escook/request-miniprogram
```

老师这个包的具体内容：

```js
class Request {
  constructor(options = {}) {
    // 请求的根路径
    this.baseUrl = options.baseUrl || ''
    // 请求的 url 地址
    this.url = options.url || ''
    // 请求方式
    this.method = 'GET'
    // 请求的参数对象
    this.data = null
    // header 请求头
    this.header = options.header || {}
    this.beforeRequest = null
    this.afterRequest = null
  }

  get(url, data = {}) {
    this.method = 'GET'
    this.url = this.baseUrl + url
    this.data = data
    return this._()
  }

  post(url, data = {}) {
    this.method = 'POST'
    this.url = this.baseUrl + url
    this.data = data
    return this._()
  }

  put(url, data = {}) {
    this.method = 'PUT'
    this.url = this.baseUrl + url
    this.data = data
    return this._()
  }

  delete(url, data = {}) {
    this.method = 'DELETE'
    this.url = this.baseUrl + url
    this.data = data
    return this._()
  }

  _() {
    // 清空 header 对象
    this.header = {}
    // 请求之前做一些事
    this.beforeRequest && typeof this.beforeRequest === 'function' && this.beforeRequest(this)
    // 发起请求
    return new Promise((resolve, reject) => {
      let weixin = wx
      // 适配 uniapp
      if ('undefined' !== typeof uni) {
        weixin = uni
      }
      weixin.request({
        url: this.url,
        method: this.method,
        data: this.data,
        header: this.header,
        success: (res) => { resolve(res) },
        fail: (err) => { reject(err) },
        complete: (res) => {
          // 请求完成以后做一些事情
          this.afterRequest && typeof this.afterRequest === 'function' && this.afterRequest(res)
        }
      })
    })
  }
}

export const $http = new Request()
```





## 使用方式

直接看代码：

```js
// 1.直接在main.js中引入
import {$http}  from '@escook/request-miniprogram'
// 2.挂载到顶级对象uni上
uni.$http = $http;

// $http.baseUrl自定义基础地址

// 3.设置请求拦截器
$http.beforeRequest = function(options) {
  uni.showLoading({
    title:'数据加载中...'
  })
}
// 4.设置响应拦截器
$http.afterRequest = function(options) {
  // 隐藏loading
  uni.hideLoading()
}

```



**支持的请求方法：**

```js
// 发起 GET 请求，data 是可选的参数对象
$http.get(url, data?)

// 发起 POST 请求，data 是可选的参数对象
$http.post(url, data?)

// 发起 PUT 请求，data 是可选的参数对象
$http.put(url, data?)

// 发起 DELETE 请求，data 是可选的参数对象
$http.delete(url, data?)
```

