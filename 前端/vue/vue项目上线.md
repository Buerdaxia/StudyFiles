## vue项目上线

第一步：首先：修改vue.config.js下的路径

```js
module.exports = {
    //添加这一条 确定静态资源路径
	publicPath: './'
};
```

运行`npm run build`将项目进行打包，会打包出一个`dist`文件。

第二步：将dist文件夹放到服务器上，可以用Xftp



第三步：配置nginx



