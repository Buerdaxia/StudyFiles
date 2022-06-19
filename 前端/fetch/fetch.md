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
  .then(data => console.log(data));
```

这里我们通过网络获取一个 JSON 文件并将其打印到控制台。最简单的用法是只提供一个参数用来指明想 `fetch()` 到的资源路径，然后返回一个包含响应结果的 promise（一个 [`Response`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response) 对象）。

当然它只是一个 HTTP 响应，而不是真的 JSON。为了获取JSON的内容，我们需要使用 [`json()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/json) 方法（该方法返回一个将响应 body 解析成 JSON 的 promise）。



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

