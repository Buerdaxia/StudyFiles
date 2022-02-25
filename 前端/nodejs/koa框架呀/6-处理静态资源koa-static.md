# 处理静态资源koa-static

## 安装

```
npm install koa-static
```



## 使用

```js
const path = require('path');
// 导入静态资源处理中间件
const koaStatic = require('koa-static');
// 注册koaStatic ，需要传一个静态资源存放路径
app.use(koaStatic(path.join(__dirname, '../uploads')));
```



之后只要后端将存放的文件名返回过来：前端就可以通过再搜索栏打入

`http://localhost:xxxx/文件名`

就可以直接访问该图片了

例如：

后端返回数据

```js
{
    "code": 0,
    "message": "图片上传成功",
    "result": {
        "goods_img": "upload_bc6d544b88fd19230854029e1c092a8c.png"
    }
}
```

服务端地址：`http://localhost:8000/`

搜索栏敲入：`http://localhost:8000/upload_bc6d544b88fd19230854029e1c092a8c.png`

可以直接访问该png图片