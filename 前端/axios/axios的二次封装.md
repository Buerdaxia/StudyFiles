# axios

基于promise的在浏览器或者nodejs下的HTTP传输，发送AJAX请求

**关键信息，操作等请看官网**

[axios (axios-js.com)](http://www.axios-js.com/docs/)



json-server启动：npx json-server --watch **db.json**(最后这个是文件名)

特点：

- Make [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) from the browser

- Make [http](http://nodejs.org/api/http.html) requests from node.js

- Make [http](http://nodejs.org/api/http.html) requests from node.js

等等...

## axios的二次封装

在src文件下面创建api文件，再创建一个js文件写入以下内容。

当然也可以看着官网进行更深入的封装

```js
//axios的二次封装
import axios from 'axios'

// 该选项能够判断是开发环境还是生产环境
let isDev = process.env.NODE_ENV === 'development'

// 1.利用axios对象的方法create，去创建一个axios实例
const requests = axios.create({
	// 基础路径
	baseURL: isDev ? '开发环境api' : '生产环境api',
	// 请求超时时间5s
	timeout: 5000
	// 请求头
	// headers: headers
})

// 请求拦截器: 在发送请求之前，请求拦截器可以拦截到，可以在请求发送出去之前做一些操作

requests.interceptors.request.use(
	config => {
		//config：配置对象，对象里有一个属性很重要，headers请求头
		// Do something before request is sent
		return config
	},
	error => {
		// Do something with request error
		return Promise.reject(error)
	}
)

// 响应拦截器：数据请求回来后可以做一些操作
axios.interceptors.response.use(
	res => {
		// 成功的回调函数
		return res.data
	},
	error => {
		// 响应失败的回调
		// Do something with response error
		return Promise.reject(error)
	}
)

// 对外暴露分装好的axios对象
export default requests

```

