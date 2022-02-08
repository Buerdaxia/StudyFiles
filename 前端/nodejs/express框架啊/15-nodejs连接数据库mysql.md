# nodejs连接数据库mysql

一般都会将连接数据库的文件放在项目目录下的`db`文件夹中的`db.js`。



首先需要安装`mysql`

```
npm install mysql;
或者
yarn add mysql;
```



第二步：在`db`文件夹中创建`db.js`文件并写入一下代码

```js
var mysql = require('mysql');

var pool = mysql.createPool({
	host: 'localhost',
	user: 'root',
	password: '123456',
	database: 'qianduan_test'
}); // 连接数据库的配置

/*
  sql: sql语句
  callback: 回调函数
*/
function query(sql, callback) {
	pool.getConnection(function(err, connection) {
		connection.query(sql, function(err, rows) {
			callback(err, rows);
			connection.release();
		});
	});
}

exports.query = query;

```



第三步：在别的文件中引入db文件并使用query函数即可

```js
const express = require('express');
// 1.引入db.js文件中的db
const db = require('./db/db.js');

const app = new express();

app.get('/get_data', (req, res) => {
	// 2.使用db暴露出来的query函数
	/*
    err: 报错信息
    data: 查询出来的数据
  */
	db.query('select * from students', (err, data) => {
		console.log(data);
		// 3.对data进行处理
		res.send(data);
	});
	// res.send('hello word');
});

app.listen(4000, () => {
	console.log('服务器以及启动, 端口为: 4000');
});

```

