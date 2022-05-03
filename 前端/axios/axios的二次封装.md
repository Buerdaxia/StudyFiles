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
import { ElMessage } from 'element-plus'
import { diffTokenTime } from '@/utils/auth'
import store from '@/store'
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
    // 进行token判断
    if (localStorage.getItem('token')) {
      if (diffTokenTime()) {
        // store下的appmodules中放了一个logout方法
        store.dispatch('app/logout')
        return Promise.reject(new Error('token 失效了'))
      }
    }
    config.headers.Authorization = localStorage.getItem('token')
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
    // 我们可以自己封装一下统一的错误处理
    const {data, meta} = res.data; 
    if(meta.status === 200 || meta.status === 201) {
      return data;
    }else {
      // 这里还可以封装一下304，301等token相关的错误处理
      // 这里用的是element-plus的message组件进行错误处理
      ElMessage.error(meta.msg)
      return Promise.reject(new Error(meta.msg))
    }
		return res.data
	},
	error => {
		// 响应失败的回调
		// Do something with response error
    // 直接进行错误判断并处理，如果有错就直接打印了
		error.response && ElMessage.error(error.response.data)
    return Promise.reject(new Error(error.response.data))
	}
)

// 对外暴露分装好的axios对象
export default requests

```



在`utils/auth.js`下的代码

```js
import { TOKEN_TIME, TOKEN_TIME_VALUE } from './constant'

// 登录时设置时间
export const setTokenTime = () => {
  localStorage.setItem(TOKEN_TIME, Date.now())
}

// 获取登录时间
export const getTokenTime = () => {
  return localStorage.getItem(TOKEN_TIME)
}

// 是否已经过期
export const diffTokenTime = () => {
  // 1.获取现在时间
  const currentTime = Date.now()
  // 2.获取登录时的时间
  const tokenTime = getTokenTime()
 	// 3.返回比较结果
  return currentTime - tokenTime > TOKEN_TIME_VALUE
}

```

`/constant`代码

```js
export const TOKEN_TIME = 'tokenTime'
// 由于时间是毫秒所以乘以1000
export const TOKEN_TIME_VALUE = 2 * 60 * 60 * 1000

```





## 关于axios传递超大数字问题

JavaScript能够处理数字的最大范围是：-2^53到 2^53之间（不含两个端点）

反正就：如果数字大小超过16位了，就注意了数字就不精确了

前后端数据传递过程中，后端返回的数据一般都是`JSON`格式的，`axios`在内部会帮助我们自动调用了以下`JSON.parse()`帮助我们将数据转换为了JavaScript对象来方便我们使用。但是在转换**大数字**时就会出现精度问题。

解决方案：

**使用第三方包`json-bigint`来解决**

安装json-bigint

```
npm install json-bigint
```



第二步，再封装的`axios`中配置一下

```js
// axios二次封装
import axios from 'axios';
// 解决特大数字传递问题
import JSONBig from 'json-bigint';

const isDev = process.env.NODE_ENV === 'development';
const requests = axios.create({
    // 配置基础url
	baseURL: isDev ? 'http://localhost:8080/api' : 'http://192.168.13.5:8689',
    // 超时时间
	timeout: 5000,
    // 解决大数字问题
	transformResponse: [
		function (data) {
			try {
                // 遇到大数字，如果能转换则转换后返回
				return JSONBig.parse(data);
			} catch (err) {
                // 否则直接返回
				return data;
			}
		}
	]
});

requests.interceptors.request.use(
	config => {
		// Do something before request is sent
		return config;
	},
	error => {
		// Do something with request error
		return Promise.reject(error);
	}
);

requests.interceptors.response.use(
	res => {
		// Do something before response is sent
		return res;
	},
	error => {
		// Do something with response error
		return Promise.reject(error);
	}
);

export default requests;

```



最后：

**使用时，只需要调用一下`toString()`就能够得到大数字的字符串形式，可以直接使用**



## 关于解决调用接口415问题

有时候要注意请求头上的`Content-Type: application/json;charset=UTF-8`配置项有时候，响应头上的`Content-Type`要求和请求头一致

一般我们可以修改我们二次分装的`axios`修改以下请求头

上代码：

```js
// axios二次封装
import axios from 'axios';

const isDev = process.env.NODE_ENV === 'development';
const requests = axios.create({
	baseURL: isDev ? 'http://localhost:8080/api' : 'http://192.168.13.5:8689',
	timeout: 5000,
    // 关键在这一行，修改了以下请求头上的Content-Type属性
	headers: { 'Content-Type': 'application/json;charset=UTF-8' }
});

requests.interceptors.request.use(
	config => {
		// Do something before request is sent
		return config;
	},
	error => {
		// Do something with request error
		return Promise.reject(error);
	}
);

requests.interceptors.response.use(
	res => {
		// Do something before response is sent
		return res;
	},
	error => {
		// Do something with response error
		return Promise.reject(error);
	}
);

export default requests;

```

## 4xx和5xx错误

一般来说4xx开头的状态码一般都时请求时的错误：

例如

```
422:一般标识请求参数格式错误
400:请求语法有问题，一般是请求参数写错了
404:接口找不到
415:可能时请求头的Content-Type有问题
405:可能是请求方法出错

500:服务器错误
```

