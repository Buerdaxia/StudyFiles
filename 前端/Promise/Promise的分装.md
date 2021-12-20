# Promise的分装

分装过程：

一般都是将AJAX,FS,等异步操作封装成一个函数，这个函数中返回的是一个`promise`对象，

`promise`回调中，有触发成功的`resolve`和失败的`reject`。

然后，再引入模块或者函数的地方，进行成功和失败回调的书写=>也就是写`then`或`catch`

例如：

## 封装node中的fs

```js
//将文件读取fs封装成函数
// 参数： path 文件路径
// 返回： promise 对象

function mineReadFile(path) {

    return new Promise((resolve, reject)=>{
        // 读取文件 require('fs')就会返回一个文件对象
        require('fs').readFile(path, (err, data)=>{
            // 失败
            if(err) reject(err);

            // 成功
            resolve(data);
        });
    });
}

//调用时：
mineReadFile('./resource/content.txt')
.then(value=>{
    console.log(value.toString());
}, reason=>{
    console.log(reason);
});
```





## 用promise进行封装uni-app中的分装请求:

```js
// 封装并暴露初去
// 返回一个promise对象
// options：传入的对象
/* options格式：
	{
	  url: 'xxx',
	  method:'xxx',
	  data: {xxx}
	}
*/
// myRequest: 暴露出去的函数

const BASE_URL = 'URL';//公共的url
export const myRequest = (options) => {
  return  new Promise((resolve, reject) => {
  	// 里面包裹异步任务，也就是请求
  	uni.request({
  	  url: BASE_URL + options.url,
  	  method: options.method || 'GET',//默认为GET
  	  data: options.data || {},//默认为空
  	  success: (res) => {
  	  	// 这个是根据接口来看的
  	  	if(res.data.status! == 0) {
  	  	  return uni.showToast({
  	  	    title: "获取数据失败"
  	  	  })
  	  	}
  	  	resolve(res);
  	  },
  	  fail: (err) => {
  	  	uni.showToast({
  	  	  title: "请求接口失败"
  	  	})
  	  	reject(err);
  	  }
  	})
  })
}
```

## 封装AJAX请求

```js
/*
 *封装一个函数 sendAjax 发送 GET AJAX请求
 *参数：url
 *返回结果：Promise对象
*/
function sendAjax(url) {
  return new Promise((resolve, reject) => {
  	const xhr = new XMLHttpRequest();
  	xhr.open('GET', 'url');
  	xhr.send();
  	xhr.onreadystatechange = function() {
  	  if(xhr.readystate === 4) {
  	    if(xhr.status >= 200 && xhr.status <= 300) {
  	      resolve(xhr.response);
  	    } else {
  	      reject(xhr.status);
  	    }
  	  }
  	}
  })
}

//使用
sendAjax('https://api.apiopen.top/getJoke')
.then(res => {
  console.log(res);
})
.catch(err => {
  console.log(err);
})
```







## util.promisify 方法（node中使用）

这个是`node.js`中的一个方法,在node中可以将原先那种回调函数风格的方法，转换成`promise`风格的方法

```js
/*
    util.promisify 风格
*/
// 引入util
const util = require('util');
// 引入fs模块
const fs = require('fs');

// 返回一个新函数 返回的结果就是一个promise对象
let mineReadFile = util.promisify(fs.readFile);

mineReadFile('./resource/content.txt')
.then(value=>{
    console.log(value.toString());
});
```

