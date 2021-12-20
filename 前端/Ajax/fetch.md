# fetch请求

语法:`.fetch(input[, init]);`

返回一个`promise`对象。

参数：

input：

* 一个 [`USVString`](https://developer.mozilla.org/zh-CN/docs/Web/API/USVString) 字符串，包含要获取资源的 URL。一些浏览器会接受 `blob:` 和 `data:` 作为 schemes.
* 一个 [`Request`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request) 对象。(通过new Request创建的后续会讲)

init：

一个配置对象，包括所有对请求的设置。

* method

