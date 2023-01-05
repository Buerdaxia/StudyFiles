# BOM对象



## location对象

location 是最有用的 BOM 对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能。 这个对象独特的地方在于，它既是 window 的属性，也是 document 的属性。也就是说， window.location 和 document.location 指向同一个对象。



假设浏览器当前加载的 URL 是 `http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=  javascript#contents`，location 对象的内容如下表所示。

| 属性                | 值                                                          | 说明                                                         |
| ------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| location.hash       | `"#contents"`                                               | URL 散列值（井号后跟零或多个字符），如果没有则 为空字符串    |
| location.host       | `"www.wrox.com:80"`                                         | 服务器名及端口号                                             |
| location.hostname   | `"www.wrox.com"`                                            | 服务器名                                                     |
| location.href       | `"http://www.wrox.com:80/WileyCDA/ ?q=javascript#contents"` | 当前加载页面的完整 URL。location 的 toString() 方法返回这个值 |
| location.pathname   | ` "/WileyCDA/"`                                             | URL 中的路径和（或）文件名                                   |
| location.port       | `"80"`                                                      | 请求的端口。如果 URL中没有端口，则返回空字符串               |
| location.protocol   | ` "http:" `                                                 | 页面使用的协议。通常是"http:"或"https:"                      |
| **location.search** | `"?q=javascript"`                                           | URL 的查询字符串。这个字符串以问号开头                       |
| location.username   | ` "foouser"`                                                | 域名前指定的用户名                                           |
| location.password   | ` "barpassword"`                                            | 域名前指定的密码                                             |
| location.origin     | `"http://www.wrox.com"`                                     | URL 的源地址。只读                                           |





### 查询字符串

其实url中我们获取最多的还是`location.search`,查询字符串携带过来的参数。原生的location.search字符串，会有问号，下面是我们手动拆分

手动拆分：

```js
let getQueryStringArgs = function() { 
 // 取得没有开头问号的查询字符串
 let qs = (location.search.length > 0 ? location.search.substring(1) : ""), 
 // 保存数据的对象
 args = {}; 
 // 把每个参数添加到 args 对象
 for (let item of qs.split("&").map(kv => kv.split("="))) { 
   let name = decodeURIComponent(item[0]), 
   value = decodeURIComponent(item[1]); // 这里使用decodeURIComponent()解码，应为一般都会被加密的
   if (name.length) { 
   args[name] = value; 
   } 
 }
   return args; 
} 
```



使用`URLSearchParams`进行拆分

URLSearchParams提供了一套标准的API，专门用于解析`location.search`的查询字符串，这个实例上暴露了 get()、 set()和 delete()等方法，可以对查询字符串执行相应操作

示例：

```js
let qs = "?q=javascript&num=10"; // 这个就相当于location.search
let searchParams = new URLSearchParams(qs); 
alert(searchParams.toString()); // " q=javascript&num=10" 
searchParams.has("num"); // true 
searchParams.get("num"); // 10 
searchParams.set("page", "3"); 
alert(searchParams.toString()); // " q=javascript&num=10&page=3" 
searchParams.delete("q"); 
alert(searchParams.toString()); // " num=10&page=3" 
```

大多数支持 URLSearchParams 的浏览器也支持将 URLSearchParams 的实例用作可迭代对象：

```js
let qs = "?q=javascript&num=10"; 
let searchParams = new URLSearchParams(qs); 
for (let param of searchParams) { 
 console.log(param); 
} 
// ["q", "javascript"] 
// ["num", "10"]
```



### 操作地址

操作浏览器的地址可以通过以下几种方法

**最常见的**：`assign()`方法给location并入一个url

示例：

```js
location.assign("http://www.wrox.com"); 
```



方法二：location.href 或 window.location 设置一个 URL

>本质上，还是会调用assign方法给location并入一个url

示例：

```js
window.location = "http://www.wrox.com"; 
location.href = "http://www.wrox.com"; 
```

**以上方法，操作地址后，都会往历史记录里插入一条记录，用户点击浏览器的后退按钮是可以进行后退的**



如果不想后退，可以使用`replace`方法

语法：

```js
location.replace('url')
// 这样“后退”按钮是禁用状态，即不能返回这个示例页面，除非手动输入完整的 URL
```



## navigator对象

navigator 是由 Netscape Navigator 2 最早引入浏览器的，现在已经成为客户端标识浏览器的标准。 只要浏览器启用 JavaScript，navigator 对象就一定存在。但是与其他 BOM 对象一样，每个浏览器都 支持自己的属性。

navigator有一大堆属性和方法，鬼鬼建议MDN一下



### 检测插件

检测浏览器是否安装了某个插件是开发中常见的需求。除 IE10 及更低版本外的浏览器，都可以通 过 plugins 数组来确定。这个数组中的每一项都包含如下属性。

* name：插件名称。
* description：插件介绍
* filename：插件的文件名
* length：由当前插件处理的 MIME 类型数量。



## screen对象

window 的另一个属性 screen 对象，是为数不多的几个在编程中很少用的 JavaScript 对象。这个对 象中保存的纯粹是客户端能力信息，也就是浏览器窗口外面的客户端显示器的信息，比如像素宽度和像 素高度。每个浏览器都会在 screen 对象上暴露不同的属性。

如果要看，可以看《JavaScript高级程序设计》p404





## history对象

history 对象表示当前窗口首次使用以来用户的导航历史记录。因为 history 是 window 的属性， 所以每个 window 都有自己的 history 对象。出于**安全考虑**，这个对象不会暴露用户访问过的 URL， 但可以通过**它在不知道实际 URL 的情况下前进和后退**。



### 导航

go()方法，可以在用户历史记录中沿任何方向导航，该方法接受一个参数，参数为一个整数。vue中的router.go方法好像用的就是这个

语法：

```js
// 后退一页
history.go(-1); 
// 前进一页
history.go(1); 
// 前进两页
history.go(2); 
```



**go()方法的两个简写方法：back()和forward()**，功能字如其名

语法：

```js
// 后退一页
history.back(); 
// 前进一页
history.forward(); 

```

