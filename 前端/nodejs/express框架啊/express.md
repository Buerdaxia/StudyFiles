# express

express是一个基于内置核心`http`模块，一个第三方的包，专注于`web服务器`的构建。

安装：

如果是一个新项目

```
yarn init -y

npm init
```

然后执行：

```
yarn add express

npm install express
```

## 基本使用

第一步：引入express

```js
const express = require('express');
```

第二步：创建项目对象

```js
const app = new express();
```

第三步：处理请求

```js
// .get表示处理get请求
app.get('/', (req, res) => {
	// req是请求对象，res响应对象

	res.send('hello express框架！');
});
```



第四步：监听

```js
app.listen(3000, err => {
	if (err) {
		console.log(err);
		return;
	}
	// 这里的代码会在服务器启动时执行
	console.log('express web server is listen in port 3000! ');
});
```

## 第一个express程序

```js
// 1.引入
const express = require('express');

// 2.创建项目对象
const app = new express();

// 3.处理请求
// .get表示处理get请求
app.get('/', (req, res) => {
	// req是请求对象，res响应对象

	res.send('hello express框架！');
});

// 4.监听是否有请求
app.listen(3000, err => {
	if (err) {
		console.log(err);
		return;
	}
	// 这里的代码会在服务器启动时执行
	console.log('express web server is listen in port 3000! ');
});

```

