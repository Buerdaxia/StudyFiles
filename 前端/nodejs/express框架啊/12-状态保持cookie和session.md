# 状态保持

## 状态保持技术cookie和session

因为http请求是一种无状态协议，浏览器请求服务器是一种无状态的。。所以需要一些状态保持技术。



无状态：指一次用户请求时，浏览器、服务器无法知道之前这个用户做过什么，每次请求都是一次全新的。



实现状态保持的两种方式：

1. 在客户端存储信息使用**Cookie**
2. 在服务器端存储信息使用**Session**



## cookie

特点：

1. cookie由服务器生成，保存在浏览器端的一小段文本信息
2. cookie事宜建值的形式进行存储
3. 浏览器在访问一个网站的服务器时，会自动在请求头中把和本网站相关的信息所有cookie发给服务器
4. cookie是基于域名安全的
5. cookie有过期时间，默认关闭浏览器后关闭

### 使用

第一步：nodejs中首先要安装`cookie-parser`中间件

```
yarn add cookie-parser
npm install cookie-parser
```



第二步：项目中引入并注册

```js
const express = require('express');
// 1.引入cookie-parser
const cookieParser = require('cookie-parser');
const app = express();
// 2.在app上注册 注意，cookieParser是个函数
app.use(cookieParser());
```

注意：这里有几个小问题

1. 引入的cookieParser是个函数，注册时要写为`app.use(cookieParser());`
2. 如果引入了外部路由，则注册`cookie-parser`中间件必须放到**注册路由代码下面**



第三步：编写设置`cookie`

```js
app.get('/set_cookie', (req, res) => {
	// 3.设置cookie 一组键值对
	/*
    'name': 是键
    'node': 是值
    maxAge: 过期时间，单位是毫秒
    如果不设置过期时间，则浏览器关闭后自动销毁cookie
  */							// 2个小时过期
	res.cookie('name', 'node', { maxAge: 60 * 60 * 2 * 1000 });
    // age cookie浏览器关闭后旧没了
	res.cookie('age', 11);
	res.send('设置了cookie信息');
});
```



第四步：获取请求头中携带的`cookie`

```js
// // 4.获取cookie信息
app.get('/get_cookie', (req, res) => {
	let name = req.cookies['name'];
	let age = req.cookies['age'];
	console.log(req.cookies);

	res.send(`获取到的cookie信息为:${name}, ${age}`);
});

```



完整代码：

```js
const express = require('express');
// 1.引入cookie-parser
const cookieParser = require('cookie-parser');
const app = express();
// 2.在app上注册 注意，cookieParser是个函数
app.use(cookieParser());

app.get('/', (req, res) => {
	res.send('express框架啊');
});

app.get('/set_cookie', (req, res) => {
	// 3.设置cookie 一组键值对
	/*
    'name': 是键
    'node': 是值
    maxAge: 过期时间，单位是毫秒
  */
	res.cookie('name', 'node', { maxAge: 60 * 60 * 2 * 1000 });
	res.cookie('age', 11);
	res.send('设置了cookie信息');
});

// // 4.获取cookie信息
app.get('/get_cookie', (req, res) => {
	let name = req.cookies['name'];
	let age = req.cookies['age'];
	console.log(req.cookies);

	res.send(`获取到的cookie信息为:${name}, ${age}`);
});

app.listen(3000, () => {
	console.log('Express服务器已经在3000端口运行');
});

```



## session

特点：

1. **session数据保存在服务器端**
2. session是以键和值的形式进行存储的
3. session依赖于cookie，每个session值信息对应的客户端的标识值



## 使用

第一步：先安装：`cookie-session`

```
yarn add cookie-session
npm install cookie-session
```



第二步：引入注册

```js
const express = require('express');
// 1.引入cookie-session
const cookieSession = require('cookie-session');
const app = express();

// 2.注册session并传入参数
app.use(
	/*
    name：session名
    keys: 为内部加密所需要的keys，随机字符串（越乱越好）
    maxAge: 过期时间，单位是毫秒
  */
	cookieSession({
		name: 'my_session',
		keys: ['abccba'],
		maxAge: 1000 * 60 * 60 * 2
	})
);
```



第三步：设置`session`键值对

```js
app.get('/set_session', (req, res) => {
	// 3.设置session信息
	req.session['name'] = 'nodejs_session';
	req.session['age'] = 12;
	res.send('设置了session数据');
});
```



第四步：获取请求头中的`session`值

通过`req.session`获取

```js
app.get('/get_session', (req, res) => {
	// 4.获取请求中的session值
	let name = req.session['name'];
	let age = req.session['age'];
	res.send(`获取到的session数据为: ${name}, ${age}`);
});
```



完整代码：

```js
const express = require('express');
// 1.引入cookie-session
const cookieSession = require('cookie-session');
const app = express();

// 2.注册session并传入参数
app.use(
	/*
    name：session名
    keys: 为内部加密所需要的keys，随机字符串
    maxAge: 过期时间，单位是毫秒
  */
	cookieSession({
		name: 'my_session',
		keys: ['abccba'],
		maxAge: 1000 * 60 * 60 * 2
	})
);

app.get('/', (req, res) => {
	res.send('express框架啊');
});

app.get('/set_session', (req, res) => {
	// 3.设置session信息
	req.session['name'] = 'nodejs_session';
	req.session['age'] = 12;
	res.send('设置了session数据');
});

app.get('/get_session', (req, res) => {
	// 4.获取请求中的session值
	let name = req.session['name'];
	let age = req.session['age'];
	res.send(`获取到的session数据为: ${name}, ${age}`);
});

app.listen(3000, () => {
	console.log('Express服务器已经在3000端口运行');
});

```

