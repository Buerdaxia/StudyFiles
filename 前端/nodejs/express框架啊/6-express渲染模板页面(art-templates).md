# express渲染模板页面(art-templates)

## 安装

先安装`art-template`和`express-art-template`

```
yarn add art-template 或 npm install art-template

yarn add express-art-template 或 npm install express-art-template
```

## 使用

这四句设置复制一下即可，官网有

注意app.set后面那个官网写的时`view`，要修改成`view options`

```js
// 1.引入
const express = require('express');
const path = require('path');

// 2.创建项目对象
const app = new express();
// 引入express-art-template,使用对应的引擎
// view engine setup
// html表示我们使用的html
app.engine('html', require('express-art-template'));

// 生产环境（线上）production
// 开发环境 development
app.set('view options', {
	debug: process.env.NODE_ENV !== 'production'
});
// 设置在哪一个目录下查找HTML文件，设置模板存放目录
app.set('views', path.join(__dirname, 'views'));

// 设置模板后缀名为html
app.set('view engine', 'html');

// 访问根目录时直接渲染index.html
app.get('/', (req, res) => {
    // 注意这里就不用写后缀名了，上面已经处理过了
	res.render('index');
});

app.listen(3000, err => {
	if (err) {
		console.log(err);
		return;
	}
	// 这里的代码会在服务器启动时执行
	console.log('express web server is listen in port 3000! ');
});

```

