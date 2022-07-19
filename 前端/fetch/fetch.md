# fetch请求

**fetch不同与ajax，axios，$.ajax，fetch是一套全新的通讯方案，而axios，$.ajax都是对原有ajax的封装**

详情请查阅：`MDN`

语法:`.fetch(input[, init]);`

返回一个`promise`对象。

参数：

input：

* 一个 [`USVString`](https://developer.mozilla.org/zh-CN/docs/Web/API/USVString) 字符串，包含要获取资源的 URL。一些浏览器会接受 `blob:` 和 `data:` 作为 schemes.
* 一个 [`Request`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request) 对象。(通过new Request创建的后续会讲)

init：

一个配置对象，包括所有对请求的设置。

* method



一个基本的`fetch`:

```js
fetch('http://example.com/movies.json')
  .then(response => response.json())
// response.json()得到的是一个promise对象
  .then(data => console.log(data));
```

这里我们通过网络获取一个 JSON 文件并将其打印到控制台。最简单的用法是只提供一个参数用来指明想 `fetch()` 到的资源路径，然后返回一个包含响应结果的 promise（一个 [`Response`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response) 对象）。

当然它只是一个 HTTP 响应，而不是真的 JSON。为了获取JSON的内容，我们需要使用 [`json()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/json) 方法（该方法返回一个将响应 body 解析成 JSON 的 promise）。



一个多一点儿的`fetch`:

```js
		// 使用fetch发送
		fetch(`http://localhost:3000/api/search/users2?q=${keyWord}`)
			.then(
				response => {
					console.log('联系服务器成功', response);
					return response.json();
				},
				err => {
					console.log('联系服务器失败', err);
          // 如果没有这一步，失败之后会接着往下调用
          return new Promise(); // 中断链式调用
				}
			)
			.then(
				res => {
					console.log('获取数据成功', res);
				},
				err => {
					console.log('获取数据失败', err);
				}
			);


// 改进版：用catch捕获错误就行了
		fetch(`http://localhost:3000/api/search/users2?q=${keyWord}`)
			.then(response => {
				console.log('联系服务器成功', response);
				return response.json();
			})
			.then(res => {
				console.log('获取数据成功', res);
			})
			.catch(err => {
				console.log(err);
			});
```



究极改进版：

```js
		try {
			const response = await fetch(`http://localhost:3000/api/search/users2?q=${keyWord}`);
			const data = await response.json();
			console.log('获取数据成功', data);
		} catch (error) {
			console.log('请求出错', error);
		}
```









可以替代axios，不过要封装一下

```js
result = await fetch('xxx',{
  headers: {
    ...
  }
  method: 'post',
  body: {
    ...
  }
}).then(res => {
  return res.json();
})
```

