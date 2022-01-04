## url模块

## 引入模块

```js
import {URL} from 'url';

//es6
const {URL} = require('url');
```



## 使用

```js
const server = http.createServer((request, response) => {
	// 获取请求路径 请求报文第一行的第二个信息
	let reqUrl = request.url;

	// 获取请求方式
	let reqMethod = request.method;

	// 获取get请求的请求参数
    // url.parse不推荐使用了
	// let obj = url.parse(reqUrl, true);
    
	const obj = new URL(reqUrl, 'http://localhost:8081/');
    // 获取携带的query参数通过get方法
	console.log(obj.searchParams.get('curPage'));
	console.log(obj.searchParams.get('perPage'));
	console.log(obj.search);

	response.write('hello Im writer');
	response.end('hello world.'); //该行代码后，不要在做响应操作了，会报错
});
```

