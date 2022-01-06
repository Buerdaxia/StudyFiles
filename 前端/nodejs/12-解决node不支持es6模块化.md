# 解决node不支持es6模块化

在node使用

```
import xxx from 'xxx'
```

这类es6模块化规范会报错。



## 解决方式

可以将代码进行转换，把es6规范转换为commonjs规范。

利用转换工具(babel-cli和browserify)



解决：

1. 在项目文件夹下生成package.json文件

```
yarn init -y 或 npm init -y
```

2. 安装第三方工具（注意：有三个babel-cli，browserify，babel-preset-es2015）

   在任意目录下执行以下代码，进行全局安装

   ```
   yarn global add babel-cli browserify
   或
   npm install babel-cli browserify -g
   ```

   在自己目录下执行：

   ```
   yarn add babel-preset-es2015
   或
   npm install babel-preset-es2015
   ```

3. 在项目根目录新建`.babelrc`文件

   ```
   {
   	"presets": [
   		"es2015"
   	]
   }
   ```

4. 在src目录下书写完毕代码后，执行：

   如果没有lib目录可以新建一个

   ```
   babel src -d lib
   //这句代码意思是将src下的代码转换以下放到lib中
   ```

5. 之后执行`lib`目录下的js代码即可

**如果执行第4条出现外部命令错误，看npm与yarn文件夹下的yarn的安装与使用，里面有解决办法**

