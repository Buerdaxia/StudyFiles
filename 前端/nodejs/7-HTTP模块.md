# HTTP模块

## 第一个服务器

```js
// 搭建第一个后端的程序
// 1.引入http模块
const http = require('http');
// 2.配置服务器程序的端口号
const port = 8081;
// 3.创建服务器对象
const server = http.createServer((request, response) => {
	// request请求对象，response响应对象
	// 这里的代码，每接受到一次请求就来执行一次这里的代码

	// write方法，往响应体中写入内容
	response.write('hello Im writer');
	// end方法也可以写入一些内容，但是表示响应工作已经结束
	response.end('hello world.'); //该行代码后，不要在做响应操作了，会报错
});

// 4.调用服务器对象的监听方法（让服务器去监听浏览器的请求）
server.listen(port, error => {
	if (error) {
		console.log(error);
		return;
	}
	console.log('webServer is listening at port 8081!');
});

```



## 获取请求路径

`request.url`通过该属性可以获取请求路径

```js
// 这里只截取server部分代码
const server = http.createServer((request, response) => {
	// 获取请求路径 请求报文第一行的第二个信息
	let reqUrl = request.url;
	console.log(reqUrl);

	response.write('hello Im writer');
	response.end('hello world.'); //该行代码后，不要在做响应操作了，会报错
});
```



## 获取请求方式

`request.method`通过该属性获取请求方式

```js
const server = http.createServer((request, response) => {
	// 获取请求路径 请求报文第一行的第二个信息
	let reqUrl = request.url;

	// 获取请求方式
	let reqMethod = request.method;
	console.log(reqUrl, reqMethod);

	response.write('hello Im writer');
	response.end('hello world.'); //该行代码后，不要在做响应操作了，会报错
});
```



## 小细节

`response.end()`表示响应结束，该函数后，不要再进行响应操作了，会报错。记住`response.end()`永远放在最后即可。



