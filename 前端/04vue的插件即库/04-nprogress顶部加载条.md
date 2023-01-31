# vue插件，nprogress

插件的作用：在顶部添加一个进度条，来显示进度。

![use](C:\Users\10854\Desktop\web前端\前端图片\nprogress\use.png)



## 插件的使用

第一步、安装插件

```
npm install --save nprogress
```



第二步、在封装好的axios中引入(因为这个插件一般都是显示请求过程，请求结束了，进度条也走完了)

```js
// 引入进度条 插件
import nprogress from 'nprogress';
// 引入进度条 css样式 该样式在node_modules下的nprogress文件夹下
// 该样式可以修改，修改这个css就好了
import 'nprogress/nprogress.css';



// 然后在响应拦截器和请求拦截器中添加俩方法
/* nprogress的
  start方法：进度条开始 在请求拦截器中设置
  done方法：进度条结束  在响应拦截器中设置
*/

requests.interceptors.request.use(
	config => {
		//config：配置对象，对象里有一个属性很重要，headers请求头
		// Do something before request is sent
		// 进度条成功
		nprogress.start();
		return config;
	},
	error => {
		// Do something with request error
		return Promise.reject(error);
	}
);

// 响应拦截器：数据请求回来后可以做一些操作
requests.interceptors.response.use(
	res => {
		// 成功的回调函数
		// 进度条结束
		nprogress.done();
		return res.data;
	},
	error => {
		// 响应失败的回调
		// Do something with response error
		return Promise.reject(error);
	}
);
```

