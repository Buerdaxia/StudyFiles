# Ajax

Ajax（Asynchronous JavaScript and XML）

能力：浏览器赋予了JavaScript主动发起网络请求的能力

### XMLHttpRequest

### 1 open



JS 要发起一个http请求，首先要**new**一个 XHR对象

![20200502213515](C:\Users\10854\Desktop\前端图片\Ajax\20200502213515.png)

有了XHR对象，需要调用open方法‘打开’XHR对象：

![20200502213525](C:\Users\10854\Desktop\前端图片\Ajax\20200502213525.png)

open方法接收两个个参数：

1. method 表示http请求类型
2. url 表示http请求地址

如果想在url中放参数和内容格式：在url后加一个问号？，参数用&分割

语法`url?a=100&b=200&c=300`

和http中的请求报文中的请求行类似

![20200502212338](C:\Users\10854\Desktop\前端图片\http协议\20200502212338.png)

注意：**调用了open方法之后并不会真正的发出请求，而是启动了一个请求以备发送**

open方法第一个参数最常见的方法是**get**请求，其次是**post**请求

他们的主要区别是：

1. get请求直接通过URL给服务端发消息，也就是将消息一并拼接在URL里![QQ截图20210712091234](C:\Users\10854\Desktop\前端图片\Ajax\QQ截图20210712091234.png)
2. post请求，约定通过body来传递信息，也就是用xhr.send(传递内容)

post数据放在：请求体

get数据放在：请求行



#### 1.1 setRequestHeader()方法 设置请求头

注意！：**该方法必须放在open()之后，send()之前**

语法`xhr.setRequestHeader('参数一','参数二');`

参数一：设置请求体内容类型

参数二：设置请求体内容字符串

例如：`xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');`





#### 1.2 xhr.responseType = 'json'

加上之后对从服务端传回的json字符串直接自动转换为js对象

json对象第一种手动转换：

```javascript
let data = JSON.parse(xhr.response);
result.innerHTML = data.name;
```



第二种自动转换：

```javascript
// 还有一种自动转换 在上面加上xhr.responseType = 'json';
result.innerHTML = xhr.response.name;
```

#### 1.3 xhr.timeout = 2000；

timeout属性设置请求最大时间，如果超过这个时间就取消请求。

#### 1.4 xhr.ontimeout = function() {};超时回调函数

改函数可以设置当请求超时之后，的后续事件。

```javascript
xhr.ontimeout = function() {
    alert("网络异常，请稍后再试");
}
```

#### 1.5 xhr.onerror = function() {}; 网络异常回调函数

该函数在当网络异常是可以设置行为。

```javascript
xhr.onerror = function() {
    alert("歪歪歪？听到到吗？你网没了！");
}
```



### 2 send 方法

如果想要真正的将请求发送出去，需要调用**send**方法，send方法只接受一个参数，就是http请求报文中的**消息体**

消息类型必须为字符串，如果不打算想服务器发送消息，也需要传一个**null**作为参数

### 3 获取服务器返回数据

首先XHR对象的三个属性：

1. **responseText**：保存了服务器返回的数据
2. **status**：服务器响应报文的状态码200，404，....
3. **readyState**：表示当前XHR对象请求/响应过程的当前活动阶段，这个属性有一下取值：![20200502213642](C:\Users\10854\Desktop\前端图片\Ajax\20200502213642.png)

一个完整的请求过程：

![InkedQQ截图20210711102845_LI](C:\Users\10854\Desktop\前端图片\Ajax\InkedQQ截图20210711102845_LI.jpg)

其中红线部分**onreadystatechange** 就是决定能否获得到服务器数据的关键所在，每当XHR对象的readyState属性发生改变时，如果onreadystatechange属性是一个函数，那么浏览器都会运行这个函数。

为了确保浏览器完全接受到了服务器发来的数据要在函数中增加判断条件：

![QQ截图20210711103535](C:\Users\10854\Desktop\前端图片\Ajax\QQ截图20210711103535.png)

上文说到之后**readyState===4**时表示**完成**完全接受数据，然后为了确保这次请求没有错误，还需要判断http的状态码必须在200-300之间，或者等于304

code：`if（200 <= xhr.status <300 || xhr.status == 304）`

![QQ截图20210711104225](C:\Users\10854\Desktop\前端图片\Ajax\QQ截图20210711104225.png)

### 4取消请求

取消请求用abort()方法,是

```javascript
let x = new XMLHttpRequest();
x.abort();//取消请求
```







# AJAX 内容

![20200503114713](C:\Users\10854\Desktop\前端图片\Ajax\20200503114713.png)



