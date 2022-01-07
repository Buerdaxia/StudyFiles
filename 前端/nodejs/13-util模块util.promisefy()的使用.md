# util.promisefy()的使用

引入util工具包

````
const util = require('util')
````

调用`util`中的`promisefy()`函数

采用遵循常见的错误优先的回调风格的函数（也就是将 `(err, value) => ...` 回调作为最后一个参数），并返回一个返回 promise 的版本。（**返回一个promise实例**）

例如异步读取文件:

```js
// 这就是一个错误优先的回调
fs.readFile(filePath, 'utf-8', (err, data) => {
 		if (err) {
 			reject(err);
 		}
		resolve(data);
	});
```

## 例如，promisefy化读取文件

这是利用`util.promisify(fs.readFile)`

```js
let readFilePromise = util.promisify(fs.readFile);

// 调用，后面的参数和readFile的前两个一致，只是没有那个回调函数了
let p1 = readFilePromise(filePath1, 'utf-8');

let p2 = readFilePromise(filePath2, 'utf-8');

let p3 = readFilePromise(filePath3, 'utf-8');
```

这是自己分装的函数：

```js
function readFilePromise(filePath) {
	return new Promise((resolve, reject) => {
		fs.readFile(filePath, 'utf-8', (err, data) => {
			if (err) {
				reject(err);
			}
			resolve(data);
		});
	});
}
```

