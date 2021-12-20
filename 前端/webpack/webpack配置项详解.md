## entry的详细配置

```js
/*
  entry: 入口起点
  1. string-->"./src/index.js"（单入口）
    打包形成一个chunk，输出一个bundle文件，此时chunk的名字默认为main

  2. array-->["./src/index", "./src/add.js"]（多入口）
    所有入口文件最终会形成一个chunk，输出还是只有一个bundle文件
    -->这种打包方式，就是在HMR功能中引入html文件让html热更新生效

  3. object { index: "./src/index.js", add: "./src/add.js"}（多入口）
    有几个入口文件就会形成几个chunk，输出几个bundle文件，此时chunk的名称为key
    注意：要修改output中filename的值，因为要打包输出多个bundle所以名字就不能固定
    写法为：[name].js（name为chunk的名字）
    -->特殊用法（dll技术就用的这种）
    {
      // 数组中的入口会形成一个chunk，输出一个bundle文件
      index: ["./src/index.js", "./src/count.js"],
      add: "./src/add.js"
    }
*/

const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: {
    index: ["./src/index.js", "./src/count.js"],
    add: "./src/add.js"
  },
  output: {
    filename: "[name].js",
    path: resolve(__dirname, "build")
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  mode: 'development'
}
```

## output的详细配置

```js

const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: "./src/index.js",
  output: {
    // 文件名称(目录+指定名称)
    filename: "js/[name].js",
      
    // 输出文件目录(将来所有资源输出的公共目录)
    path: resolve(__dirname, "build"),
      
    // 所有资源引入公共路径的前缀 -->"images/a.jpg" --> "/images/a.jpg"（一般用于生产环境）
    // 在打包好的文件中，引入资源加的前缀
    publicPath: "/",
      
    // 非入口chunk的名称（不是entry引入的）
    // 例如import动态引入的，code-split分割出的module
    chunkFilename: "js/[name]_chunk.js",
      
    // 将整个库以name的名称暴露出去(和libraryTarget一起使用的)--->dll中使用了
    library: "[name]",
    // 变量名添加到哪里去
    // libraryTarget: "window"//变量名添加到browser上
    // libraryTarget: "global"//变量名添加到node上
    libraryTarget: "commonjs"//以commonjs方式暴露
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  mode: 'development'
}
```

## module中loader的详细配置

```js

const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "js/[name].js",
    path: resolve(__dirname, "build")
  },
  module: {
    rules: [
      // loader配置
      {
        // 检查某些文件
        test: /\.css$/i,

        // 多个loader用use
        use: ["style-loader", "css-loader"]
      },
      {
        test: /\.css$/i,
        // enforce: "pre",//优先执行
        // enforce: "post",//延后执行
        
        // 排除node_modules下的js文件
        exclude: /node_modules/,

        // 只检查src下的js文件
        include: resolve(__dirname, "src"),

        // 单个loader用loader
        loader: 'eslint-loader'
      },
      {
        // 以下配置只会生效一个
        oneOf: []
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  mode: 'development'
}
```

## resolve（简化操作）

```js

const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: "./src/js/index.js",
  output: {
    filename: "js/[name].js",
    path: resolve(__dirname, "build")
  },
  module: {
    rules: [
      // loader配置
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  mode: 'development',
  // 解析模块的规则
  resolve: {
    // 配置解析模块的别名: 简写路径（好用的一批）
    alias: {
      // $css就是 存css的绝对路径
      $css: resolve(__dirname, "src/css")
    },
    // 配置省略文件路径名的后缀名(注意：文件名不能重名)
    extensions: [".js", ".json", ".css"],
    // 告诉webpack 解析模块时去找那个目录(也可以直接写绝对路径)
    modules: [resolve(__dirname,"../../node_modules"),"node_modules"]
  }
}
```



## devServer（webpack4的）

```js
  devServer: {
    // 运行代码目录
    contentBase: resolve(_dirname, "build"),
    // 监视contentBase 目录下的所有文件，一旦文件变化就会reload
    watchContentBase: true,
    // 启动gzip压缩
    compress: true,
    // 端口
    port: 5000,
    // 域名
    host: "localhost",
    // 开启HMR
    hot: true,
    // 自动打开浏览器
    open: true,
    // 除了基本启动信息之外，其他内容不要提示
    quiet: true,
    // 出错了，不要全屏提示，在日志中
    overlay: false,
    // 解决跨域问题的好方法
    proxy:{
      // 一旦devServer(5000)服务器接收到/api/xxx的请求，就会把请求转发到另一个服务器(3000)上
      '/api': {
        target: "http://localhost:3000",
        // 发送请求时，请求路径重写：将/api/xxx -> /xxx（去掉/api）
        pathRewrite: {
          "^/api": ''
        }
      }
    }
  }
```

## optimization(webpack4的)

主要看最下面optimization的配置

```js

const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const TerserWebpackPlugin = require('terser-webpack-plugin');
module.exports = {
  entry: "./src/js/index.js",
  output: {
    filename: "js/[name].[contenthash:10].js",
    path: resolve(__dirname, "build"),
    chunkFilename: "js/[name].[contenthash:10]_chunk.js"
  },
  module: {
    rules: [
      // loader配置
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  mode: 'development',
  // 解析模块的规则
  resolve: {
    alias: {
      $css: resolve(__dirname, "src/css")
    },
    extensions: [".js", ".json", ".css"],
    modules: [resolve(__dirname,"../../node_modules"),"node_modules"]
  },
  optimization: {
    splitChunks: {
      chunks: "all",
      // 以下为默认配置(一般不用修改)
      /*
      minSize: 30*1024,//分割chunk最小为30kb
      maxSize: 0,//最大无限制
      minChunks: 1,//要提取的chunk最少被引用1次
      maxAsyncRequests: 5, //按需加载时并行加载文件的最大数量
      maxInitialRequests: 3, //入口js文件最大并行请求数量为3个
      automaticNameDelimiter: "~", //命名连接符
      name: true, //可以使用命名规则
      cacheGroups: {
        // 分割chunk的组
        // node_modules文件会被打包到vendors组的chunk中 --> vendors~xxx.js
        // 满足上面的公共规则，如：大小超过30kb，至少被引用一次
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          // 要提取的chunk最少被引用2次
          minChunks: 2,
          // 优先级
          priority: -20,
          // 如果当前打包的模块，和之前被提取的模块是同一个，就会复用，否则重新打包
          reuseExistingChunk: true
        }
      }*/
    },
    // 解决缓存失效问题
    // 解决修改a文件导致b文件contenhash改变的方法
    runtimeChunk: {
      name: entrypoint => `runtime-${entrypoint.name}`
    },
    minimizer: [
      // 配置生产环境的压缩方案： js和css
      new TerserWebpackPlugin({
        // 开启缓存
        cache: true,
        // 开启多进程打包
        parallel: true,
        // 启动source-map
        sourceMap: true
      })
    ]
  }
}
```

