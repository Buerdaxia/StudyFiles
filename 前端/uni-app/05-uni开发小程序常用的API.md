# uni常用的API

## 第一个：获取手机信息uni.getSystemInfoSync()

获取一些系统信息，同步接口。

例如手机品牌，手机屏幕尺寸，手机屏幕高度等等等

更多信息，可以去官网查看





## 第二个：图片预览 uni.previewImage(OBJECT)

调用这个接口可以进行图片的预览操作，原生小程序应该也有类似的API。

具体的参数，请看官网



## 第三个：设置tabBar的徽标uni.setTabBarBadge

这个属性，可以给tabBar右上角添加一个小徽标（就是一个小圆点儿，里面有一个数字）

具体的配置请看uni的官网，原生小程序也有该方法





## 第四个：uni.chooseAddress()打开微信小程序选择地址原生界面

这个API需要在app.json中要重新配置一下，具体配置可以去官网查看

调用这个api直接就可以有获取地址的界面了，界面中包含添加地址等功能（需要真机调试才能看到，小程序模拟器上显示为默认的张三）

示例使用：

```js
// 触发这个函数，就会弹出选择地址界面
async chooseAddress() {
  // err是报错信息，succ是选择地址后返回的结果
  const [err, succ] =  await uni.chooseAddress().catch(err => err);
  // succ下面有一个errMsg如果选择地址成功了，值为"chooseAddress:ok"
  if(err === null && succ.errMsg === "chooseAddress:ok") {
    // 调用user的mutations存储address
    this.updateAddress(succ);
  }
}
```

