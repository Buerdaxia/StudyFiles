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



## ORM

ORM全拼Object-Relation Mapping--对象-关系映射

主要实现模型对象到关系数据库的映射

一条记录映射为一个对象，一个字段映射为一个属性



## ORM的使用

首先在`db`文件夹下创建`nodejs-orm`文件夹，并创建一个`index.js`文件，写入以下代码：

```js
const mysql = require('mysql');// 没有，则需要安装
// 数据库连接设置 要修改的位置
let orm_config = {
    host: 'localhost',//数据库地址
    port:'3306',
    user: 'root',//用户名，没有可不填
    password: '',//密码，没有可不填
    database: 'qianduan_test'//数据库名称
}

let options = {};
let tableSQL = '';
let isConnect = false;

function Model(name, option) {
    this.name = name;
    this.option = option;
};

/**
* @description: 查询数据
* @param {} options：可选参数
* @param {Function} callback :（req,results）=>{}
*/
Model.prototype.find = function (options, callback) {
    if (!isConnect) {
        console.log(options.constructor);
        this.connect(err => {
            isConnect = true;
            var str = '';
            if (!callback) {
                str = `select * from ${this.name}`;
                callback = options;
            }else if (options.constructor == Array) {
                str = `select ${options.join()} from ${this.name}`;
            }else if(options.constructor == Object){
                str = `select ${options.arr.join()} from ${this.name} where ${options.where}`;
            }else {
                str = `select * from ${this.name} where ${options}`;
            };
            //console.log(str);
            connection.query(str, (error, results, fields) => {
                callback(error, results, fields);
            });
            return this;
        })
    } else {
        
        var str = '';
        if (!callback) {
            str = `select * from ${this.name}`;
            callback = options;
        } else if (options.constructor == Array) {
            str = `select ${options.join()} from ${this.name}`;
        } else {
            str = `select * from ${this.name} where ${options}`;
        };
        //console.log(str);
        connection.query(str, (error, results, fields) => {
            callback(error, results, fields);
        });
        return this;
    }

};

/**
* @description: 分页查询
* @param {Object} options :   { where:查询条件, number: 当前页数 , count : 每页数量 }
* @return: 
*/
Model.prototype.limit = function (options, callback) {
    var str = '';
    if (!options.where) {
        str = `select * from ${this.name} limit ${(options.number - 1) * options.count},${options.count}`;
    } else {
        str = str = `select * from ${this.name} where ${options.where} limit ${(options.number - 1) * options.count},${options.count}`;
    };
    console.log(str);
    connection.query(str, (error, results, fields) => {
        callback(error, results, fields);
    });
    return this;
};

/**
* @description: 插入数据
* @param {Object} obj:对象或者数组
* @param {Function} callback :（req,results）=>{}
*/
Model.prototype.insert = function (obj, callback) {
    if (!isConnect) {
        this.connect(err => {
            if (err) {
                throw err;
            } else {
                connection.query(tableSQL, (error, results, fields) => {
                    if (Array.isArray(obj)) {
                        for (var i = 0; i < obj.length; i++) {
                            this.insertObj(obj[i], callback)
                        }
                    } else {
                        this.insertObj(obj, callback)
                    }
                });

            }
        });
    } else {
        if (Array.isArray(obj)) {
            for (var i = 0; i < obj.length; i++) {
                this.insertObj(obj[i], callback)
            }
        } else {
            this.insertObj(obj, callback)
        }
    }

};

Model.prototype.insertObj = function (obj, callback) {
    let keys = [];
    let values = '';
    for (var key in obj) {
        keys.push(key);
        values += `"${obj[key]}",`;
    };
    values = values.replace(/,$/, '');
    let str = `INSERT INTO ${this.name} (${keys.join()}) VALUES (${values})`;
    connection.query(str, (error, results, fields) => {
        callback(error, results);
    });
}

/**
* @description: 更新数据
* @param {Object} option：可选参数 更新条件
* @param {Object} obj： 修改后的数据 
* @param {Function} callback :（req,results）=>{}
*/
Model.prototype.update = function (option, obj, callback) {
    let str = '';
    if (arguments.length == 2) {
        callback = obj;
        obj = option;
        str = `UPDATE ${this.name} SET `;
        for (var key in obj) {
            str += `${key}='${obj[key]}', `;
        };
        str = str.replace(/(, )$/, '');
    } else {
        str = `UPDATE ${this.name} SET `;
        for (var key in obj) {
            str += `${key}='${obj[key]}', `;
        };
        str = str.replace(/(, )$/, '');
        str += ` where ${option}`;
    };

    console.log(str);
    connection.query(str, (error, results, fields) => {
        callback(error, results, fields);
    });
    return this;

};

/**
* @description: 删除数据
* @param {Object} option：可选参数 删除条件
* @param {Function} callback :（req,results）=>{}
*/
Model.prototype.delete = function (option, callback) {
    var str = '';
    if (!callback) {
        str = `delete from ${this.name}`;
        callback = option;
    } else {
        str = `delete from ${this.name} where ${option}`;
    };
    console.log(str);
    connection.query(str, (error, results, fields) => {
        callback(error, results, fields);
    });
    return this;
};

/**
* @description: 执行sql语句
* @param {String} str : sql语句
* @param {Function} callback :（req,results）=>{}
*/
Model.prototype.sql = function (str, callback) {
    connection.query(str, (error, results, fields) => {
        callback(error, results, fields);
    });
    return this;
};

/**
* @description: 删除model表格 （慎用！）
* @param {type} 
* @return: 
*/
Model.prototype.drop = function (callback) {
    connection.query(`DROP TABLE ${this.name}`, (error, results, fields) => {
        callback(error, results, fields);
    });
    return this;
};

//连接检测
Model.prototype.connect = function (callback) {
    let p1 = new Promise((resolve, reject) => {
        connection.connect((err) => {
            if (err) {
                //console.log(err.stack);
                //console.log(err);//42000 数据库不存在  28000账号错误
                //console.log(err.sqlState);//42000 数据库不存在  28000账号错误
                reject(err);
            } else {
                resolve();
            }
        });
    });

    p1.then(() => {
        callback(null);
    }, err => {
        if (err.sqlState == 42000) {
            createDatabase(callback);
        } else if (err.sqlState == 28000) {
            callback('数据库账号或密码错误');
        } else {
            callback(err);
        }
    });
};

//创建数据库
let createDatabase = function (callback) {
    let p2 = new Promise((resolve, reject) => {
        connection = mysql.createConnection({
            host: options.host,//数据库地址
            port: options.port,//端口号
            user: options.user,//用户名，没有可不填
            password: options.password,//密码，没有可不填
        });
        connection.connect((err) => {
            //if (err) throw error;
            if (err) {
                reject(err);
            } else {
                resolve();
            }
        });
    });

    let p3 = new Promise((resolve, reject) => {
        connection.query(`CREATE DATABASE ${options.database}`, (err, results, fields) => {
            //if (error) throw error;
            if (err) {
                reject(err);
            } else {
                resolve();
            }

        });
    });

    let p4 = new Promise((resolve, reject) => {
        connection.query(`use ${options.database}`, (err, results, fields) => {
            if (err) {
                reject(err);
            } else {
                resolve();
            }
        });
    });

    let pAll = Promise.all([p2, p3, p4]);

    pAll.then(() => {
        callback(null);
    }).catch((err) => {
        callback(err);
    });
}



let orm = {
    /**
    * @description:连接数据库
    * @param {String} host: 主机名 默认localhost
    * @param {Number} port: 端口号 默认3306
    * @param {String} user: 用户名 
    * @param {String} password: 密码 
    * @param {String} database: 数据库名称 默认og
    * @return: 
    */
    connect: function ({ host = 'localhost', port = 3306, user = '', password = '', database = 'og' }) {
        databaseName = database;//全局存储当前数据库名称

        options = {
            host,//数据库地址
            port,//端口号
            user,//用户名，没有可不填
            password,//密码，没有可不填
            database//数据库名称
        };
        connection = mysql.createConnection(options);

    },
    /**
    * @description:创建model (表格模型对象)
    * @param {String} name:表格名称
    * @param {Object} options:表格数据结构
    * @return: Model对象：负责数据库增删改查
    */
    model: function (name, options) {
        let str = 'id int primary key auto_increment, ';
        for (var key in options) {
            if (options[key] == Number) {
                str += `${key} numeric,`;
            } else if (options[key] == Date) {
                str += `${key} timestamp,`;
            } else {
                str += `${key} varchar(255),`;
            }
        };
        str = str.replace(/,$/, '');
        //console.log(`CREATE TABLE ${name} (${str})`);
        //console.log(str);
        tableSQL = `CREATE TABLE ${name} (${str})`;
        return new Model(name, options);
    }
};

orm.connect(orm_config);



module.exports = orm;
```



第二步：使用时将上面的index.js文件引入

```js
const express = require('express');
// 1.引入orm
const db = require('./db/nodejs-orm/index');

const app = new express();

app.get('/get_data', (req, res) => {
	// 2.创建模型,填入查询那一张表

	let Students = db.model('students');

	// 3.使用find方法
	/*
    参数一：数组，里面填写要查询的字段
    参数二：回调函数，查询的数据和出错信息
  */
	Students.find(['name', 'age'], (err, data) => {
		res.send(data);
	});
	// res.send('orm的使用');
});

app.listen(3000, () => {
	console.log('服务器以及启动, 端口为: 3000');
});

```





## ORM的查询操作

orm的查询操作关键依赖于模型对象的`find()`方法

语法：

`模型对象.find([xx], (err, data) => {})`

### 全部查询

相当于`select * from xxx`

```js
// 创建模型对象
let Students = db.model('students');
/*
	不写参数一，直接查询所有
*/
Students.find((err, data) => {
	res.send(data);
});
```



### 指定字段查询

在`find`方法中添加数组，数组值为要查询指定字段名

```js
// 创建模型对象
let Students = db.model('students');
/*
	参数一：数组，数组内容为指定字段名
*/
Students.find(['name', 'age'], (err, data) => {
	res.send(data);
});
```



### 按条件查询

相当于sql中的`where 条件`，以字符串方式书写参数一

```js
// 创建模型对象
let Students = db.model('students');
/*
	参数一：字符串，指定查询条件
*/
Students.find('age > 18', (err, data) => {
	res.send(data);
});
```



### 分页查询

```js
// 创建模型对象
let Students = db.model('students');
/*
	参数一：为对象
	where: 查询条件，可选项
	number: 第几页
	count: 每页条数
*/
Students.limit({where: 'age> 18',number: 1, count: 5}, (err, data) => {
	res.send(data);
});
```

### 扩展(组合条件查询)

```js
// 创建模型对象
let Students = db.model('students');
/*
	参数一：为对象
	where: 查询条件，可选项
	arr: 指定查询字段
*/
Students.limit({where: 'age> 18',arr: ['name', 'age']}, (err, data) => {
	res.send(data);
});
```



## ORM增加数据

添加数据时，使用`insert()`方法

### 单条数据添加

```js
// 创建model对象
let Students = db.model('students');

// 单条插入
Students.insert({ name: '赵云', age: 20 }, (err, data) => {
	res.send(data);
    
    /*
    	data输出结果是一个对象
    	"insertId":15 =>是插入时的id
    	{"fieldCount":0,"affectedRows":1,"insertId":15,"serverStatus":2,"warningCount":0,"message":"","protocol41":true,"changedRows":0}
    */
});
```



### 多条数据添加

```js
// 创建model对象
let Students = db.model('students');
	// 多条数据插入
	Students.insert(
		[
			{ name: '钱不二', age: 22 },
			{ name: '刘备', age: 33 },
			{ name: '关羽', age: 34 }
		],
		(err, data) => {
			console.log(data);
		}
	);
	res.send('添加成功');
});
```



## ORM删除数据

删除数据使用的时`delete()`方法

注意：该删除为物理删除，直接将数据库内容删除

### 删除单条数据

```js
// 创建model对象
let Students = db.model('students');
/*
	第一个参数：字符串，删除数据的标识
*/
Students.delete('id=21', (err, data) => {
	console.log(data);
});
res.send('删除成功');
```

### 删除整张表

```js
// 创建model对象
let Students = db.model('students');
/*
	不传递第一个参数，则会直接删除整个model对象对应的表
*/
Students.delete((err, data) => {
	console.log(data);
});
res.send('删除成功');
```



## ORM修改数据

修改数据使用`update()`方法

### 全部修改

修改整张表的所有字段

```js
// 创建model对象
let Students = db.model('students');
// 全部字段修改
/*
  第一个参数是一个对象：属性为要修改的字段
*/
Students.update({ age: 30 }, (err, result) => {
	console.log(result);
});
res.send('修改成功');
```



### 根据条件进行修改

```js
// 创建model对象
let Students = db.model('students');
/* 
    第一个参数：字符串，修改条件
    第二个参数：对象，修改字段和值
    第三个参数：回调
*/
Students.update('id=1', { age: 18 }, (err, result) => {
  console.log(result);
});
res.send('修改成功');
```



## ORM自定义sql语句

当前面的增删改查不够用时，可以自定义sql语句进行查询

```js
// 创建model对象
let Students = db.model('students');
/*
  第一个参数：字符串，自定义的sql语句
*/
Students.sql('select * from students where id >= 10', (err, data) => {
  res.send(data);
});
```

