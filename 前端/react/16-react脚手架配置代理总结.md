# react脚手架配置代理总结



## 方法一

> 在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，**当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）**



注意：发送请求时，我们要修改我们自己的发送信息的接口，将接口前半部分修改为和脚手架自己开的服务器统一地址：

例如：

```jsx
// 原接口
axios.get('http://localhost:5000/students')

// 修改为（假如react脚手架服务器开在：http://localhost:3000）
axios.get('http://localhost:3000/students')
```





## 方法二

现在`http-proxy-middleware`中间件需要自己安装了

>第0步：安装`http-proxy-middleware`

```js
npm install http-proxy-middleware
// or
yarn add http-proxy-middleware
```



1. 第一步：创建代理配置文件

   ```
   在src下创建配置文件：src/setupProxy.js
   ```

2. 编写setupProxy.js配置具体代理规则：（**配置了两个代理**）

   ```js
   const proxy = require('http-proxy-middleware')
   
   // 使用commonJS方式
   module.exports = function(app) {
     app.use(
       proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
         target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
         changeOrigin: true, //控制服务器接收到的请求头中host字段的值
         /*
         	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
         	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
         	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
         pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
       }),
       proxy('/api2', { 
         target: 'http://localhost:5001',
         changeOrigin: true,
         pathRewrite: {'^/api2': ''}
       })
     )
   }
   
   
   // 新版本： 新脚手架使用以上版本会导致打不开问题的出现
   const { createProxyMiddleware } = require('http-proxy-middleware');
   
   module.exports = function(app) {
   	// 第一个代理
     app.use(
   		'/api',
   		createProxyMiddleware({
   			target: 'http://localhost:5000',
   			changeOrigin: true,
   			pathRewrite: { '^/api': '' }// 重写请求路径
   		})
   	);
   	// 第二个代理
   	app.use(
   		'/api2',
   		createProxyMiddleware({
   			target: 'http://localhost:5001',
   			changeOrigin: true,
   			pathRewrite: { '^/api2': '' }
   		})
   	);
   };
   
   
   
   ```

说明：

1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。

