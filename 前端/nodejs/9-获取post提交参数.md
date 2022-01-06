# 获取post提交参数

## 通过response的data事件获取

html部分

```html
<form action="http://localhost:8081" method="post">
    用户名:<input type="text" name="username"></input><br><br>
    密&emsp;码:<input type="password" name="password"></input><br><br>
    <input type="submit" value="提交"></input>
  </form>
```

node部分：

```js
const { URL } = require('url');
const http = require('http');
const port = 8081;
const server = http.createServer((request, response) => {
	// 获取用户的用户名和密码
	// 本质就是在获取请求参数

	// 以事件方式来接受，事件名是data
	request.on('data', postData => {
		// 这里代码的执行时机？
		// 一旦接收post就会触发这里的代码
        //postData是一个buffer对象，调用toString一下
		console.log(postData.toString());
	});

	response.end('hello world.'); //该行代码后，不要在做响应操作了，会报错
});

server.listen(port, error => {
	if (error) {
		console.log(error);
		return;
	}
	console.log('webServer is listening at port 8081!');
});

```

