# dotenv中间件

作用：主要作用用来修改`process.env`文件。

他会根据根目录下的`.env`文件（自己创建一个.env文件）中的内容，向`process.env`文件中插入键值对



## 安装

```
npm install dotenv
yarn add dotenv
```

## 使用

>一：首先在根目录（和`package.json`同级）下创建一个`.env`文件，并写入键值对

```
APP_PORT=7777
// 这里以后还可以写很多配置例如：mysql的name，password呀等等...
```

>二：然后再创建`src/config/config.default.js`

config.default.js代码：

```js
const dotenv = require('dotenv');
const path = require('path');
// path.join拿到.env的路径
dotenv.config({ path: path.join(__dirname, '../../.env') });

console.log(process.env.APP_PORT);
// 将process.env暴露出去
module.exports = process.env;

```

>三：再入口文件中引入配置的**键值对**

main.js

```js
const Koa = require('koa');
// 解构赋值出.env中配置的端口
const { APP_PORT } = require('./config/config.default');
const app = new Koa();

app.use(ctx => {
	ctx.body = 'hello Koa';
});

// 这里直接就可以直接使用App_PORT
app.listen(APP_PORT, () => {
	console.log(`server is running on http://localhost:${APP_PORT}`);
});

```

