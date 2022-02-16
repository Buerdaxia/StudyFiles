# Koa框架

## 安装

```
npm install koa
或
yarn add koa
```



## 编写服务程序

1. 导入koa包
2. 实例化app对象
3. 编写中间件
4. 启动服务，监听xxxx端口

```js
// 第一个koa程序
// 一、导入Koa
const Koa = require('koa');
// 二、实例化对象
const app = new Koa();
// 三、编写中间件
app.use(ctx => {
	/* ctx:http请求的上下文
     ctx: context
  */
	ctx.body = 'hello koa';
});
// 四、启动服务

app.listen(3000, () => {
	console.log('server is running on http://localhost:3000');
});

```

