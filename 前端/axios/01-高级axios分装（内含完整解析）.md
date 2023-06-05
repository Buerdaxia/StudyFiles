# 高级axios分装（内涵完整解析）



## 基础分装

首先：我们先简单分装以下`axios`

```JS
import axios from 'axios';
/*
	customOptions:为配置化对象，用来配置是否要loading，是否取消重复请求等操作。
*/
function myAxios(axiosConfig, customOptions) {
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

测试时：可以将网速调慢一点儿，要不速度太快，看不到效果。

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



**axios版本0.22以下的封装(包括0.22)：**

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
	return [url, method, JSON.stringify(params), JSON.stringify(data)].join('&');
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



****

**axios版本0.22以上的封装：**

官网：从 `v0.22.0` 开始，Axios 支持以 fetch API 方式—— [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) 取消请求：

```js
//  判断重复请求
const pendingMap = new Map();

// const controller = new AbortController();
/**
 *
 * @param {*} config
 * return string
 * 生成每一个独特的键名
 */
function getPendingKey(config) {
	let { url, method, params, data } = config;

	if (typeof data === 'string') data = JSON.parse(data);

	// 拼接成"www.xxx&get$xxx${xx}"这样形式的字符串作为键名
	return [url, method, JSON.stringify(params), JSON.stringify(data)].join('&');
}

/**
 * 储存每个请求唯一值, 也就是cancel()方法, 用于取消请求
 * @param {*} config
 */
function addPending(config) {
  // 根据config获取map的键名
	const pendingKey = getPendingKey(config);
	// 创建一个controller对象
	const controller = new AbortController();
	// fetch方式取消请求
	config.signal = config.signal || controller.signal;
	if (!pendingMap.has(pendingKey)) {
    // 如果是一个新请求，则往map中添加一个
		pendingMap.set(pendingKey, controller);
	}
}
```





### 删除重复请求，并删除队列项

先查找是否map中是否拥有重复的请求，如果有，取消掉，并删除map中的请求

**axios版本0.22以下的封装：(包括0.22)**

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

---

**axios版本0.22以上的封装：**

示例：

```js
/**
 * 删除重复请求
 * @param {*} config
 */
function removePending(config) {
	const pendingKey = getPendingKey(config);
	if (pendingMap.has(pendingKey)) {
		// 1.得到controller对象
		const controller = pendingMap.get(pendingKey);
		// 2.调用controller对象身上的abort方法,取消请求
		controller.abort();
		// 3.从队列中删除，然后将它从队列中删除
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
			// 1.请求发送之前,先删除一下队列(Map)中的请求，防止重复请求进入队列(Map)
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



### 配置化

在封装的请求函数中，添加第二个参数，用来指定是否要开启重复请求取消。

```js
// axios.js
// customOptions配置对象
function myAxios(axiosConfig, customOptions={}) {
	const service = axios.create({
		baseURL: 'http://localhost:5173', // 设置统一的请求前缀
		timeout: 10000 // 设置统一的超时时长
	});
	
  // 合并一下配置对象，并配置默认条件
	let custom_options = Object.assign(
		{
			repeat_request_cancel: true // 默认情况开启重复请求取消
		},
		customOptions // 这个是传递进来的配置对象
	);

	// 请求拦截器，请求发送之前调用
	service.interceptors.request.use(
		config => {
			// 1.请求发送之前,先尝试删除一下，取消重复请求
			removePending(config);
			// 2.由于是请求之前，直接将请求添加进队列(进行判断，如果开启了重复请求取消，则添加进队列)
			custom_options.repeat_request_cancel && addPending(config);
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

	// 将配置对象传递给service
	return service(axiosConfig);
}

//  判断重复请求
const pendingMap = new Map();
// const controller = new AbortController();
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



接口使用时：

```js
// goods.js
export function getListAPI(paramsList) {
  return myAxios({
    url: '/api/list',
    method: 'get',
    params: paramsList
  }, {
    repeat_request_cancel: false // 传递第二个参数，配置对象
  })
}
```





## 配置Loading

首先，我们配置loading层，有以下几点核心我们需要仔细考虑：

1. 如果同一时间内，发起多个请求，我们只需要展示1个Loading层就行了，不需要重复创建多个Loading实例
2. 同一时间，多次请求，必须是最后一个请求结束之后，才能关闭Loading层
3. Loading功能需要配置化

接下来我们看这几个核心问题如何解决：

使用的是Element-plus的loading组件。

具体代码：

```js
import axios from 'axios';
// 注意如果是自动导入，这段引入代码就不用写了
import { ElLoading } from 'element-plus';

// **核心一：先创建一个对象保存loading实例，和请求接口数
const LoadingInstance = {
	_target: null, // loading实例
	_count: 0 // loading加载层数
};
/*
	customOptions：判断是否需要加载
	loadingOptions：加载的配置(具体可以取element-plus的loading去看)
*/
// axios.js
function myAxios(axiosConfig, customOptions, loadingOptions) {
	const service = axios.create({
		baseURL: 'http://localhost:5173', // 设置统一的请求前缀
		timeout: 10000 // 设置统一的超时时长
	});

	let custom_options = Object.assign(
		{
			loading: false // 默认不开启加载效果
		},
		customOptions // 这个是传递进来的配置对象
	);

	// 请求拦截器，请求发送之前调用
	service.interceptors.request.use(
		config => {

			//** 判断是否需要加载loading
			if (custom_options.loading) {
				LoadingInstance._count++; // 累加loading层数
        
        //** 这里就解决了上述问题的第一点：只创建一个Loading实例 只有=1时才创建一个实例，其他时候不变
				if (LoadingInstance._count === 1) {
					LoadingInstance._target = ElLoading.service(loadingOptions); // 调用ElLoading.service创建一个加载实例，这个加载实例身上有close方法来关闭加载
				}
			}
			return config;
		},
		error => {
			return Promise.reject(error);
		}
	);

	// 响应拦截器，请求响应完毕时调用
	service.interceptors.response.use(
		response => {
      // 接口响应完毕，关闭loading层
      custom_options.loading && closeLoading(custom_options);
			return response;
		},
		error => {
			// 报错了也一样，也是算请求响应完毕
			error.config && removePending(error.config);
      // 报错也是接口响应完毕，关闭loading层
			custom_options.loading && closeLoading(custom_options);
			return Promise.reject(error);
		}
	);

	// 将配置对象传递给service
	return service(axiosConfig);
}

/**
 * 关闭loading层
 */
function closeLoading(_options) {
  //**  这里我们解决了上述的第二个核心问题，只有到最后一个接口请求完毕之后，才会调用close()函数来关闭loading层
  // 如果count是大于零的，我们就让他减一，直到为0
  if(_options.loading && LoadingInstance._count > 0) LoadingInstance._count--;

  if(LoadingInstance._count === 0) {

    // 调用实例身上关闭方法，关闭loading层
    LoadingInstance._target.close();
    // 关闭之后，重置_target
    LoadingInstance._target = null;
  }
}



export default myAxios;

```



**提示：关于上述_count属性的解释**

`_count`属性，是对发起请求数的一个记录作用，例如：同时发送了两个请求，`_count`属性自增到2，当一个请求关闭后，会让`_count`减一，但是还不能关闭loading层(**只有_count为0时，才关闭Loading层**)，因为第二个请求还没有结束，所以当第二个请求结束时，`_count`再减一变为0了，才选择关闭loading层

当然如果你是杠精那么你又会想如果这个接口是个响应时间比较长，而且获取的数据其实并不影响页面的其他操作，那么一直有个Loading层反而是体验差了。还好我有Plan B，故设计上面的Loading层是个可配置的选项，对于这种情况的API可以选择不用这个页面级别的Loading层，转而自己去具体内使用元素级别的Loading效果更佳。





## 错误处理

一个良好的错误处理是十分有必要的，技能帮助前端定位错误，又能帮助测试人员。

示例：

```js
/**
 * 处理异常
 */
function httpErrorStatusHandle(error) {
	if (error.message === 'canceled') return console.error('重复请求');
	let message = '';

	// 判断接口的status，来修改message
	if (error && error.response) {
		switch (error.response.status) {
			case 302:
				message = '接口重定向了！';
				break;
			case 400:
				message = '参数不正确！';
				break;
			case 401:
				message = '您未登录，或者登录已经超时，请先登录！';
				break;
			case 403:
				message = '您没有权限操作！';
				break;
			case 404:
				message = `请求地址出错: ${error.response.config.url}`;
				break; // 在正确域名下
			case 408:
				message = '请求超时！';
				break;
			case 409:
				message = '系统已存在相同数据！';
				break;
			case 500:
				message = '服务器内部错误！';
				break;
			case 501:
				message = '服务未实现！';
				break;
			case 502:
				message = '网关错误！';
				break;
			case 503:
				message = '服务不可用！';
				break;
			case 504:
				message = '服务暂时无法访问，请稍后再试！';
				break;
			case 505:
				message = 'HTTP版本不受支持！';
				break;
			default:
				message = '异常问题，请联系管理员！';
				break;
		}
	}
	// 判断超时
	if (error.message.includes('timeout')) message = '网络请求超时！';
	// 判断断网
	if (error.message.includes('Network'))
		message = window.navigator.onLine ? '服务端异常！' : '您断网了！';

	ElMessage({
		message,
		type: 'error'
	});
}
```



使用：

最后再响应拦截器的err中调用一下即可

```js
	// 响应拦截器，请求响应完毕时调用
	service.interceptors.response.use(
		response => {
			// 由于接口已经响应完毕，可以将map队列中对应的请求直接删除掉
			removePending(response.config);

			// 接口响应完毕，关闭loading层
			custom_options.loading && closeLoading(custom_options);
			return response;
		},
		error => {
			// 报错了也一样，也是算请求响应完毕
			error.config && removePending(error.config);

			// 报错也是接口响应完毕，关闭loading层(应为)
			custom_options.loading && closeLoading(custom_options);

			// **这里：调用报错函数！！！
			httpErrorStatusHandle(error);
      
			return Promise.reject(error);
		}
	);
```



## 携带token

携带token就基本上不用讲了吧，调用一个携带token的函数就行啦

```js
// axios.js
import {getTokenAUTH} from '@/utils/auth';
function myAxios(axiosConfig, customOptions, loadingOptions) {
  ...
  service.interceptors.request.use(
    config => {
      ...
      // 自动携带token
      if (getTokenAUTH() && typeof window !== "undefined") {
        config.headers.Authorization = getTokenAUTH();
      }
      return config;
    }, 
    ...
  );
  ...
}

// auth.js
const TOKEN_KEY = '__TOKEN';
export function getTokenAUTH() {
   return localStorage.getItem(TOKEN_KEY);
}

```





这位大佬的作品，只是简单学习了一下罢了┭┮﹏┭┮

作者：橙某人
链接：https://juejin.cn/post/6968630178163458084
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
