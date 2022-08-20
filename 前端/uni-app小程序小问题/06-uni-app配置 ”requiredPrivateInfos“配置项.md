# uni-app配置 ”requiredPrivateInfos“配置项
在调用和地址相关的wx原生API时，现在需要在app.json中配置一下。



用uni-app开发小程序时，这个配置项的修改流程

在manifest.json ---> 源码展示 --->小程序小关，然后配置一下

示例：

```js
    "mp-weixin" : {
        /* 小程序特有相关 */
        "appid" : "wxf9ac3b15836cfa98",
        "setting" : {
            "urlCheck" : false,
            "checkSiteMap" : false
        },
        "usingComponents" : true,
        "requiredPrivateInfos": [ // 这一个新加的
          "chooseAddress" // 调用wx.chooseAddress现在要配置一下了
        ]
    },
```

