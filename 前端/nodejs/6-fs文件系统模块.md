# fs文件系统模块

## 同步读取(sync)

读取文件时，必须等到文件读取完毕才会执行后续代码

```js
// 引入fs模块
const fs = require('fs');
// 引入path模块
const path = require('path');
let filePath = path.join(__dirname, 'hello.txt');
// 同步读取
// fs.readFileSync(文件路径, '转换编码')
const content = fs.readFileSync(filePath, 'utf-8');
// 由于是同步读取，所以一下代码必须等待文件读取完毕后才能执行
console.log(content);

console.log('end-------');

```





## 异步读取(async)

不必等待，就会执行后续代码

```js
// 引入fs模块
const fs = require('fs');
// 引入path模块
const path = require('path');
let filePath = path.join(__dirname, 'hello.txt');
// 同步读取
// fs.readFileS(文件路径,'转换编码', fallback)
const content = fs.readFile(filePath, 'utf-8', (err, data) => {
	//err 是错误信息，如果没有发生错误就是null
	if (err) {
		console.log(err);
		return;
	}
	console.log(data);
});
// end---会先输出，不必等待文件读取完毕
console.log('end-------');

```



## 异步写入

`fs.writeFile(文件路径, 写入内容, '编码', fallback回调)`该方法会直接重写文件（**不是追加**）

```js
const fs = require('fs');
const path = require('path');

const filePath = path.join(__dirname, 'hello.txt');

// fs.writeFile(文件路径, 写入内容, '编码', fallback回调)
fs.writeFile(filePath, 'hello write', 'utf-8', (err, data) => {
	if (err) {
		console.log(err);
		return;
	}
	console.log('写入成功');
});

```

## 获取文件名数组

```js
// 获取当前文件夹下所有文件名数组
let nameList = fs.readdirSync(__dirname);
console.log(nameList);

//['文件名1', '文件名2', '文件名3'...]
```



## 修改文件名

```js
// 修改文件名
fs.renameSync('旧文件名', '新文件名');
```



