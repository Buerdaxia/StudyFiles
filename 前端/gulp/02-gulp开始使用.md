# gulp开始使用

## 创建目录结构

首先我们要有两个目录src和dist目录

src是我们的源码，dist是打包好的文件



必须和src同级准备一个配置文件`gulpfile.js`文件



## 安装gulp

注意：这里安装的gulp和刚刚外面的gulp不是一个。

这里安装的gulp是以第三方模块的形式出现的，由于我们需要使用gulp的语法来编写gulpfile配置文件，所以我们要下一个gulp包，来引入然后使用。



区别：

全局依赖环境gulp：

一台电脑只用安装一次，以后就可以在命令行中使用gulp xxx来运行gulp指令。



项目依赖第三方gulp：

每个项目需要引入一个gulp依赖，依赖中提供gulp语法规则，让我们来编写gulpfile.js，每个项目都需要安装一遍



安装：

```js
npm install --save-dev gulp
// 由于gulp只是开发时候打包工具而已，装到开发依赖就行了
```



## gulp的常用API

### gulp.task

语法：

```js
gulp.task(任务名称, 任务处理函数)
```

作用：创建一个基于流的任务

示例：

```js
gulp.task('htmlHandler', function() {
  // 找到html源文件，压缩，打包放入指定目录
})
// 创建一个htmlHandler任务，之后用gulp 命令行来执行这个任务就能触发它后面的处理函数
```







### gulp.src

作用：找到源文件

语法：gulp.src(路径)

示例：（常用的路径指示方式）

```js
// 1. 找到指定文件
gulp.src('./src/index.js')

// 2. 找到指定目录下，指定后缀的文件
gulp.src('./src/*.html')

// 3. 找到指定陌路下所有文件
gulp.src('./src/css/**') // **所有文件

// 4. 找到src目录下所有子目录的所有文件
gulp.src('./src/**/*')


// 5. 找到src所有子目录下，指定后缀(.html)的所有文件
gulp.src('./src/**/*.html')
```



### gulp.dest

作用：把一个内容放入指定目录内

语法：

```js
gulp.dest(路径信息)
```

示例：

```js
gulp.dest('./dist')
// 把他接收到的内容放到dist目录下
```



### gulp.watch

作用：监控指定目录下的文件，一旦发生变化，从新执行后面的任务

语法：

```js
gulp.watch(路径信息, 任务名称)
// 这里的任务是gulp.task创建的哦
```



示例：

```js
gulp.watch('./src/pages/*.html', htmlHandler);

// 当指定的目录下的html文件发生改变，就立即调用htmlHandler任务
```



### gulp.series

语法：

```js
gulp.series(任务1, 任务2, 任务3...)
```

作用：逐个执行多个任务，前一个任务执行完毕，执行第二个任务以此进行...



### gulp.parallel

语法：

```js
gulp.parallel(任务1, 任务2, 任务3)
```

作用：并行开始多个任务



### pipe重要

语法：

```js
pipe(任务)
```

作用：用于将流任务，从一个阶段，进行到下一个阶段（特别像Koa中的那个中间件思想）

示例：

```js
gulp.src(xxx).pipe(压缩任务).pipe(转码).pipe(gulp.dest(目录))
/*
	1. gulp.src获取一个文件流
	2. 通过pipe进入压缩任务这个功能，然后又返回一个流
	3. 进入转码任务,再次返回一个流
	4. 最后进入dest方法，将流放入一个指定目录
*/
```

