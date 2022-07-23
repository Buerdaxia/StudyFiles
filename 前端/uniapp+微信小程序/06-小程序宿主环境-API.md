# 小程序宿主环境-API

解释：小程序中的`API`是由宿主环境提供的，通过这些丰富的小程序`API`，开发者可以方便的调用微信提供的能力，例如：获取用户信息、本地存储、支付功能。



## 事件监听`API`

特点：以on开头，用来监听某些事件的触发

例如：

`wx.onWindowResize(function callback)`监听窗口尺寸变化的事件



## 同步`API`

特点：

1. 同步API都是以Sync结尾的
2. 同步API的指向结果，可以通过函数返回值直接获取，如果执行出错会抛出异常

例如：

`wx.setStorageSync('key','value')`向本地存储中写入内容



## 异步`API`

特点：类似于jQuery中的$.ajax(options)函数，需要通过success、fail、complete接收调用的结果。



示例：

`wx.request()`发起网络数据请求，通过success回调函数接收数据