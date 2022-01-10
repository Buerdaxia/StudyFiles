# express获取post请求参数

利用body-parser可以更方便快捷的获取到参数。

**现在不需要引入body-parser了，已经有官方中间件了**

安装：

```
yarn add body-parser

npm install body-parser
```

## 使用方式

关键:  `app.use()`方法使用中间件

第一步：注册官方中间件在项目对象上

```js
const app = new express();
// 1.使用express官方中间件
// extended为false表示使用字符串，true为qs任意值
app.use(express.urlencoded({ extended: false }));
app.use(express.json()); // 解析json格式
```

第二步：在post请求响应回调中使用`req.body`获取参数（req.body是一个对象）

```js
	// 2.获取参数 通过req.body获取参数是一个对象
	console.log(req.body);
	
	// 结构一下，并打印
	let { username, email, password, repwd } = req.body;
	console.log(username);
	console.log(email);
	console.log(password);
	console.log(repwd);
```

