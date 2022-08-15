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

Axios中对这个.abort()方法进行了分装，叫做`CancelToken`，详情请查找官网。

```js
var CancelToken = axios.CancelToken;
var cancel;


axios.get('/user/12345', {
  // 就是这里给请求添加了一个cancel函数
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```

简单理解就是通过 `new axios.CancelToken()`给每个请求带上一个专属的`CancelToken`，之后会接收到一个`cancel()` 取消方法，用于后续的取消动作，所以我们需要对应的存储好这个方法。





