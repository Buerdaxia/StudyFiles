# uni-app中的本地存储

在浏览器中有两个可以进行本地存储的好地方：

1. sessionStorage
2. localStorage





在小程序中，也有以可以进行本地存储的地方：`storage`

使用方式：和上面那俩差不多

```js
// 存储
uni.setStorageSync(键名, 键值)

// 获取
uni.getStorageSync(键名)

// 清空
uni.removeStorageSync(键名)
```

