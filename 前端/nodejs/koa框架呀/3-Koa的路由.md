# Koa的路由

路由：

* 建立URL和处理函数之间的对应关系

主要作用：**根据不同的Method和URL返回不同的内容**

示例:（原版核心思想）

```js
app.use(ctx => {
	console.log(ctx);
	// 1. ctx.request -> http请求
	// 2. ctx.response -> http响应
    
    // 可以简写为ctx.url == '/'
	if (ctx.request.url == '/') {
		ctx.response.body = '这是主页';
        // 可以简写为ctx.body = '这是主页'
	} else if (ctx.request.url == '/users') {
		if (ctx.request.method == 'GET') {
			ctx.response.body = '这是用户页';
		} else if (ctx.request.method == 'POST') {
			ctx.response.body = '创建用户';
		} else {
			ctx.response.status = 405;
		}
	} else {
		ctx.response.status = 404;
	}
});
// GET请求/，返回'这是主页'
// GET请求/users，返回'这是用户页'
// POST请求/users, 返回'创建用户'
```

访问不同的url返回不同的数据

## 使用Koa-router

### 1）安装

```
npm install koa-router
```

## 2）使用

再Koa的基础上

1. 导入`koa-router`包
2. 实例化router对象
3. 使用router处理路由
4. 注册中间件

```js
// 一,导入Koa包
const Koa = require('koa');
// 二,实例化app对象
const app = new Koa();

// 三,使用koa-router
// 3.1引入路由包
const Router = require('koa-router');
// 3.2实例化路由对象
const router = new Router();
// 3.3编写路由文件
router.get('/', ctx => {
	ctx.body = '这是主页';
});

router.get('/users', ctx => {
	ctx.body = '这是用户页';
});

router.post('/users', ctx => {
	ctx.body = '创建页面';
});
// 四,使用中间件
app.use(router.routes());
// 让koa-router支持报405和501错误
app.use(router.allowedMethods());


// 原版自带的路由
// app.use(ctx => {
// 	console.log(ctx);
// 	// 1. ctx.request -> http请求
// 	// 2. ctx.response -> http响应
// 	if (ctx.request.url == '/') {
// 		ctx.response.body = '这是主页';
// 	} else if (ctx.request.url == '/users') {
// 		if (ctx.request.method == 'GET') {
// 			ctx.response.body = '这是用户页';
// 		} else if (ctx.request.method == 'POST') {
// 			ctx.response.body = '创建用户';
// 		} else {
// 			ctx.response.status = 405;
// 		}
// 	} else {
// 		ctx.response.status = 404;
// 	}
// });

app.listen(3000, () => {
	console.log('server is running on port 3000');
});

```

## 3）优化

可以将每一个模块单独抽离出来，放到一个单独的文件中，分离出一个`router`层

例如：创建`src/router/user.route.js`

user.route.js文件:

```js
// 三,使用koa-router
// 3.1引入路由包
const Router = require('koa-router');
// 3.2实例化路由对象
const router = new Router();
// 3.3编写路由文件
router.get('/users', ctx => {
	ctx.body = '这是用户页';
});

router.post('/users', ctx => {
	ctx.body = '创建用户';
});

// 将router暴露出去
module.exports = router;

```

入口文件代码：

```js
// 一,导入Koa包
const Koa = require('koa');
// 二,实例化app对象
const app = new Koa();

// 三,导入自己编写好的router路由
const router = require('./router/user.route');
// 四,使用中间件,简写
app.use(router.routes()).use(router.allowedMethods());
// 让koa-router支持报405和501错误
// app.use(router.allowedMethods());

app.listen(3000, () => {
	console.log('server is running on port 3000');
});

```

