# express路由抽取

路由和vue中的路由概念很像，都是将每个路径拆分成一个小的文件出来，然后注册到app中



## 操作：

第一步：创建文件夹及文件

首先在项目下创建一个routes文件夹，并在文件夹下创建一个index.js和一个passport.js文件



第二步：

将之前写好的接口拉到对应的js文件中

passport.js

```js
// 引入express
const express = require('express');
// 创建router
const router = express.Router();
const path = require('path');
const fs = require('fs');

router.get('/register', (req, res) => {
	let filePath = path.join(__dirname, '../views', 'register.html');

	// 读取文件内容
	let content = fs.readFileSync(filePath, 'utf-8');

	// 访问/register时返回注册页面
	res.send(content);
});

router.post('/register', (req, res) => {
	// 一般post提交过来，需要获取到请求参数

	// 2.获取参数 通过req.body获取参数是一个对象
	console.log(req.body);

	// 结构一下，并打印
	let { username, email, password, repwd } = req.body;
	console.log(username);
	console.log(email);
	console.log(password);
	console.log(repwd);

	// 准备重定向 点击注册按钮跳到login页面
	// res.send('post');
	res.redirect('/login');
});

// 展示登录页面
router.get('/login', (req, res) => {
	let filePath = path.join(__dirname, '../views', 'login.html');

	// 读取文件内容
	let content = fs.readFileSync(filePath, 'utf-8');

	// 访问/register时返回注册页面
	res.send(content);
});

// commonjs导出规范
module.exports = router;

```



第三步：在入口文件中(每一个项目只有一个入口文件)，**引入并注册到app对象上**

app.js：

```js
const express = require('express');
const path = require('path');
const fs = require('fs');

// 引入路由文件
const passportRouter = require('./routes/passport');
const indexRouter = require('./routes/index');

const app = new express();
// 使用express官方中间件
app.use(express.urlencoded({ extended: false }));
app.use(express.json()); // 解析json格式

// 将路由对象注册到app下
app.use(passportRouter);
app.use(indexRouter);

app.listen(3000, err => {
	if (err) {
		console.log(err);
		return;
	}
	console.log('express web server is listen in port 3000! ');
});

```



