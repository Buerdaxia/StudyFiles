# 高级axios分装（内涵完整解析）



## 基础分装

首先：我们先简单分装以下`axios`

```JS
import axios from 'axios';

function myAxios(axiosConfig) {
  // 调用create函数进行自定义axios
	const service = axios.create({
		baseURL: 'http://localhost: 8888',
		timeout: 10000
	});

	return service(axiosConfig);
}

export default myAxios;

```



## post请求参数序列化

现在POST请求中的`Content-Type`一般都是以下三种：

- Content-Type: application/json
- Content-Type: application/x-www-form-urlencoded
- Content-Type: multipart/form-data

默认情况，都是用的第一个`Content-Type: application/json`，Axios也是一样，默认以这种工作方式进行传递数据，直接将参数丢尽`data`配置项就行了：

```js
import myAxios from './axios';


export function login(paramsList) {
  return myAxios({
    url: '/api/login',
    method: 'post',
    data: paramsList // 例如这里，直接丢尽data配置项，会转换为json格式
  });
}

作者：橙某人
链接：https://juejin.cn/post/6968630178163458084
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



**但是，如果后端要求`Content-Type`为application/x-www-form-urlencoded，那么就需要首先将请求头headers修改以下，其次要将数据改成键值对儿编码形式**

核心：`transformRequest`配置项，允许在向服务器发送前，修改请求数据，但只能用在 'PUT'，'POST' 和 'PATCH' 这几个请求方法，且后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream。更多使用方法，查看Axios官网。

```js
// user.js
import myAxios from './axios';

export function loginAPI(paramsList) {
  return myAxios({
    url: '/api/login',
    method: 'post',
    data: paramsList,
    headers: {
      // 一：先修改请求头
      'Content-Type': 'application/x-www-form-urlencoded'
    },
    transformRequest: [
      // 二：修改data中的数据，修改成 a=b&c=d这样的形式
      (data) => {
        let result = ''
        for (let key in data) {
          result += encodeURIComponent(key) + '=' + encodeURIComponent(data[key]) + '&'
        }
        return result.slice(0, result.length - 1)
      }
    ],
  });
}

作者：橙某人
链接：https://juejin.cn/post/6968630178163458084
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



### 使用qs模块来序列化参数

>第一步：引入qs

```
npm install qs
```

`qs`模块有俩方法：

`qs.parse()`：

```js
// 将字符串转换为对象
const data = 'name=tome&id=10';
console.log(qs.parse(data));
// {name: 'tome', id: '10'}
```

`qs.stringif()`:

```js
// 将字符串转换为对象
const data = { name: 'tom', age: 22 };
console.log(qs.stringify(data));
// name=tom&age=22
```

直接通过，`qs.stringif()`方法，转换data中的数据行了

```js
// user.js
import myAxios from './axios';
import qs from 'qs';
export function loginAPI(paramsList) {
  return myAxios({
    url: '/api/login',
    method: 'post',
    data: paramsList,
    headers: {
      // 一：先修改请求头
      'Content-Type': 'application/x-www-form-urlencoded'
    },
    transformRequest: [
      // 二：修改data中的数据，修改成 a=b&c=d这样的形式
      (data) => {
        return qs.stringify(data)
      }
    ],
  });
}
```





## 取消重复请求

首先：取消重复请求这个是很有必要的，因为一些订单呀，按钮呀等重复性操作，我们都不希望它能够重复的发送到后端进行处理。

`XMLHttpRequest` 对象是我们发起一个网络请求的根本，在它底下有怎么一个方法 `.abort()`，就是中断一个已被发出的请求。

方法1：`CancelToken`注意注意此方法官方表明`v0.22.0`已经弃用

Axios中对这个.abort()方法进行了分装，叫做`CancelToken`，详情请查找官网。

```js
var CancelToken = axios.CancelToken;
var cancel;


axios.get('/user/12345', {
  // 首先给每个请求添加一个cancelToken属性
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c; // 这个c就是取消的函数
  })
});

// 取消请求
cancel();
```

简单理解就是通过 `new axios.CancelToken()`给每个请求带上一个专属的`CancelToken`，之后会接收到一个`cancel()` 取消方法，用于后续的取消动作，所以我们需要对应的存储好这个方法。



方法2：从 `v0.22.0` 开始，Axios 支持以 fetch API 方式—— [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) 取消请求：

代码：

```js
const controller = new AbortController();

axios.get('/foo/bar', {
   signal: controller.signal
}).then(function(response) {
   //...
});
// 取消请求
controller.abort()
```



### 核心思路

1. 首先将接口状态还是pending状态的时候，将这个接口存储进队列中。

2. 如果相同的接口再一次被触发，删除并取消原来队列中相同的接口。再重新发起请求放入队列中。

3. 如果接口已经返回结果了，也将接口从队列中删除即可。



### 判断重复并存储进队列

如何判断接口一样？这里大佬是这样判断的，如果接口的请求地址，**请求方式，请求参数都一致**，那么就认为这两个接口是一样的。

存储方式：以键值对儿的方式，为了简化查找操作，我们可以使用`Map`

键名：每一个请求的标识

键值：每一个请求的cancel函数



axios版本0.22以下的封装：

```js
//  判断重复请求
const pendingMap = new Map();

/**
 *
 * @param {*} config
 * return string
 * 生成每一个独特的键值
 */
function getPendingKey(config) {
	let { url, method, params, data } = config;

	if (typeof data === 'string') data = JSON.parse(data);

	// 拼接成"www.xxx&get$xxx${xx}"这样形式的字符串作为键名
	return [url, method, params, data].join('&');
}

/**
 * 储存每个请求唯一值, 也就是cancel()方法, 用于取消请求
 * @param {*} config
 */
function addPending(config) {
	const pendingKey = getPendingKey(config);

	config.cancelToken =
		config.cancelToken ||
		new axios.CancelToken(cancel => {
			if (!pendingMap.has(pendingKey)) {
				// 如果pendingMap中没有对应的键名，那么就存储一份
				pendingMap.set(pendingKey, cancel);
			}
		});
}
```



### 删除重复请求，并删除队列项

先查找是否map中是否拥有重复的请求，如果有，取消掉，并删除map中的请求

axios版本0.22以下的封装：

```js
/**
 * 删除重复请求
 * @param {*} config
 */
function removePending(config) {
	const pendingKey = getPendingKey(config);
	if (pendingMap.has(pendingKey)) {
		// 1.得到取消请求的函数
		const cancel = pendingMap.get(pendingKey);
		// 2.调用函数取消接口发送请求
		cancel();
		// 3.从队列中删除
		pendingMap.delete(pendingKey);
	}
}
```



### 在拦截器中使用

要想在接口pending时添加进map队列，当然放到axios自带的接口中咯。

```js
import axios from 'axios';

// axios.js
function myAxios(axiosConfig) {
	const service = axios.create({
		baseURL: 'http://localhost:5173', // 设置统一的请求前缀
		timeout: 10000 // 设置统一的超时时长
	});

	// 请求拦截器，请求发送之前调用
	service.interceptors.request.use(
		config => {
			// 1.请求发送之前,先尝试删除一下，取消重复请求
			removePending(config);
			// 2.由于是请求之前，直接将请求添加进队列
			addPending(config);
			return config;
		},
		error => {
			return Promise.reject(error);
		}
	);

	// 响应拦截器，请求响应完毕时调用
	service.interceptors.response.use(
		response => {
			// 由于接口已经响应完毕，可以将map队列中对应的请求直接删除掉
			removePending(response.config);
			return response;
		},
		error => {
			// 报错了也一样，也是算请求响应完毕
			error.config && removePending(error.config);
			return Promise.reject(error);
		}
	);

	return service(axiosConfig);
}
```

