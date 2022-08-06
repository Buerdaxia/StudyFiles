# fetch的简单封装

一般将一些工具函数都放在`utils`文件夹下

## fetch请求封装

在`src`下创建`utils`文件夹，并写入以下代码：

```js
import { getJwtToken } from '../apis/auth';

// 注意：这里的的第二个参数，一定要给默认值为一个空对象
export async function request(url, { method = 'GET', body, headers, auth = true } = {}) {
	const res = await fetch(url, {
		method,
		headers: {
			'Content-Type': 'application/json',
			...(auth && { Authorization: `Bearer ${getJwtToken()}` }),
			...headers
		},
		...(body && { body: JSON.stringify(body) })
	});
	// if (res.status < 300) {
	const result = await res.json();
	return result;
	// }
	// } catch (error) {
	//   throw error;
	// }
}

```





## 跟业务请求相关的函数

一般将和业务相关的函数放在`apis`文件夹下

获取`jwt`和存储`jwt`又是和登录相关，所以在apis文件夹下创建`auth.js`

```js
// 读出jwt
export function getJwtToken() {
	const token = window.localStorage.getItem('jwtToken');
	return token;
}

// 写入jwt
export function setJwtToken(jwt) {
	localStorage.setItem('jwtToken', jwt);
}

```

