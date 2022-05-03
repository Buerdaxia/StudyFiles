# VUE-CLI

command line interface

脚手架

## 关于cli中不同版本的Vue：

1）vue.js与vue.runtime.xxx.js的区别：

（1）vue.js是完整版的Vue，包含：**核心功能+模板解析器**。

（2）vue.runtime.xxx.js是运行版的Vue，只包含：核心功能，**没有模板解析器**

2）因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容



## 使用vue.config.js配置文件

1）使用命令 vue inspect > output.js可以查看到Vue脚手架的默认配置。

2）在文件中添加vue.config.js可以对脚手架进行个性化定制，详情见https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE





## 使用cli创建项目

切换到要创建项目的文件夹之后执行：`vue create xxx`

启动项目：`npm run serve`

查看vue-cli版本：`vue --version`

方式二：

利用vue ui创建



## 脚手架创建项目文件夹详解

node_modules文件夹：项目依赖文件夹

public文件夹：一般放置一些静态资源（图片）

**需要注意，放在public文件夹中的静态资源，webpack会原封不动的打包到dist文件夹中。**



src文件夹（程序员源代码文件夹）：

​	assets文件夹：一般也是放置静态资源（一般放置多个组件共用的静态资源），需要注意的

​	该文件夹下的静态资源会被webpack打包到js文件中。

​	components文件夹：一般放置非路由组件（全局组件）

​	App.vue：唯一根组件

​	main.js：项目的入口文件



babel.config.js文件：配置文件（babel相关的）

package.json文件：里面记录项目信息。项目的身份证儿

package-lock.json：缓存性的文件

README.md：说明性文件

## Vue脚手架配置代理

**方法一：**

在vue.config.js中配置如下

本机的服务器：`http://loacalhost:8080`

```js
devServer: {
  // 写要请求的服务器地址（只写协议，主机名，端口号）
  //例如接口为：http://192.168.31.1:8080/students
  proxy: 'http://192.168.31.1:8080'//将信息发送给这个服务器
}
```



配置完毕后重启，就会开启一个代理服务器，这个代理服务器就会去向`http://192.168.31.1:8080`请求数据



所以你发请求，是发给这个代理服务器的（**代理服务器协议主机名端口号和vue开的服务器一置**）。

```js
//写请求时
axios.get('http://localhost:8080/students')
```



说明：

1. 优点：配置简单，请求资源时直接发给前端8080即可。
2. 缺点：不能配置多个代理，不能理货的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在资源时，那么改请求会转发给服务器（优先匹配前端资源）



**方式二**：

编写vue.config.js中配置如下

```javascript
devServer: {
  proxy: {
   	//这个前缀需要加到你请求服务器端口号后面
      /*例如：
      原先请求为：http://127.0.0.1:8080/router/xxx
      请求时改为：http://localhost:8080/api/router/xxx
      将请求前缀跟在端口号后 
       */
    // /api是请求前缀，如果有这个前缀就代理(可以修改/haha也行)
    '/api': {
        
      //代理的目标基础路径（修改为请求服务器的地址）
      target: 'http://127.0.0.1:8080',
      
       // 将所有/api转换为空字符串
      pathRewrite:{'^/api':''},
      ws: true,//用于支持 websocket
      changeOrigin: true//
    },
    '/demo': {
      target: 'http://localhost:5001',
      // 将所有/api转换为空字符串
      pathRewrite:{'^/demo':''},
      ws: true,//用于支持 websocket
      changeOrigin: true//如果是true,代理服务器会伪装成和服务器一样的端口号
    }
  }
}
```

理解：

一、我们发送请求是发给代理服务器的，代理服务器自动帮我们转发加接收内容，然后再通过代理服务器响应数据给我们。

二、灵活控制：就是在要跨域的请求url中加入请求前缀(写在端口号后)

三、要重写一下，请求路径，因为二的原因，请求路径都会加上一个前缀(在代理服务器请求主服务器时，也会加上的这前缀)**注意：我们的接口url中没有这个前缀，这个前缀是我们为了标识而自己加的**，就会导致找不到对应数据。所以利用`pathRewrite:{'^/前缀':''}`将前缀去掉就行啦。

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加上前缀。

