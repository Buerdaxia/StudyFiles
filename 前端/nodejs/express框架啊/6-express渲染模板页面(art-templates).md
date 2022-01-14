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
// 引入express-art-template,使用对应的引擎
// view engine setup
// html表示我们使用的html
app.engine('html', require('express-art-template'));

// 生产环境（线上）production
// 开发环境 development
app.set('view options', {
	debug: process.env.NODE_ENV !== 'production'
});
// 设置在哪一个目录下查找HTML文件
app.set('views', path.join(__dirname, 'views'));

// 设置模板后缀名为html
app.set('view engine', 'html');

// 访问根目录时，直接就可以展示页面
app.get('/', (req, res) => {
	res.render('index');
});
```

