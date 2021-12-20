# webpack性能优化（重要的一批）

## 开发环境性能优化

1. 优化打包构建速度
   * HMR(模块热替换)
2. 优化代码调试
   * source-map

## 生产环境性能优化

1. 优化打包构建速度
   * oneOf
   * babel缓存(第二次构建打包速度更快)
   * 多进程打包
2. 优化代码运行的性能
   * 缓存(hash-chunkhash-contenthash)
   * tree shaking(生产模式下和es6模块引入自动开启)
   * code split(代码分割)
   * 懒加载/预加载
   * pwa
   * externals
   * dll

## HMR

在devServer中添加`hot: true`配置项即可开启HMR热模块替换，但是开启后有一些相应的问题需要解决，以下为解决方案

### webpack4HMR热模块替换

```js
/*
  HMR: hot module replacement 热模块替换
    作用：当一个模块变化时，只会重写打包这一个模块（而不是打包所有模块）
    
    样式文件：可以使用HMR功能，style-loader再内部实现了~
    js文件：默认下js代码不能使用HMR功能
      解决：需要修改js代码
// 一旦module.hot为true则说明开启了HMR功能
if (module.hot) {
  // 方法会监听print.js文件的变化，一旦发生变化，其他默认不会重写打包构建
  // 会执行后面的回调函数
  module.hot.accept('./print.js', function() {
    print();
  })
}
      注意：HMR功能对js功能处理不能对入口文件处理，只能对其他模块进行处理
    
    html文件：默认下不能使用HMR功能，并且开启HMR后，html文件不能热更新了(单HTML不需要HMR)
      解决：修改entry入口，将html文件写入
      
*/

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  //模式
  mode: 'development',
  //改成数组，写入多个文件，热更新
  entry: ['./js/index.js', './src/index.html'],
  //出口文件
  output: {
    filename: 'js/built.js',
    path: path.resolve(__dirname, 'built')
  },
  module: {
    rules: [
      {
        //处理css文件
        test: /\.css$/i,
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.less$/i,
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      {
        // 有个问题：默认处理不了html中引入的img图片
        // 处理图片资源
        test: /\.(jpg|png|gif)$/,
        // 当只有一个loader时，可以不用use
        // 使用url-loader需要file-loader
        loader: 'url-loader',
        options: {
          // 限制图片大小小于8kb，就会被base64处理
          // 优点：减少请求数量(减轻服务器压力)
          // 缺点：图片体积会更大(文件请求时间变长)
          limit: 8* 1024,
          // esModule: false 配置项是关闭一个模块的es6模块化，使用commonJS去解析

          //[hash:10]取图片的hash的前10位
          //[ext]取文件本身的后缀名
          name: '[hash:10].[ext]',
          // 指定输出路径（是在，输出路径中的子目录）
          outputPath: 'images'
        }
      },
      {
        // 处理html中img元素
        test: /\.html$/i,
        loader: 'html-loader'
      },
      {
        // 打包其他资源
        // 排除掉我们处理过的资源，其他都打包
        exclude: /\.(css|js|html|less|png|jpg|gif)$/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          // 指定输出路径（是在，输出路径中的子目录）
          outputPath: 'media'
        }
      }
    ]
  },
  plugins: [
    //处理html文件的
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  // 开发服务器 devServer：自动化（自动编译，自动打开浏览器，自动刷新浏览器）
  // 特点：只会再内存中编译打包，不会有任何输出
  // 如果要用需要下载webpack-dev-server
  // 启动devServer指令位：npx webpack-dev-server
  devServer: {
    // 项目构建后的路径
    contentBase: path.resolve(__dirname, 'built'),
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    
    // 是否自动打开浏览器
    // open: true

    // 开启HMR功能，热模块替换
    hot: true
  }
}
```

### webpack5配置HMR

**注意：webpack5中要多下载一个raw-loader才能再entry入口中写html文件，进而才能热更新html文件**

```JS
/*
    样式文件：可以使用HMR功能，style-loader再内部实现了~
    js文件：默认下js代码不能使用HMR功能
      解决：需要修改js代码
// 一旦module.hot为true则说明开启了HMR功能
if (module.hot) {
  // 方法会监听print.js文件的变化，一旦发生变化，其他默认不会重写打包构建
  // 会执行后面的回调函数
  module.hot.accept('./print.js', function() {
    print();
  })
}
      注意：HMR功能对js功能处理不能对入口文件处理，只能对其他模块进行处理
    
    html文件：默认下不能使用HMR功能，并且开启HMR后，html文件不能热更新了(单HTML不需要HMR)
      解决：下载raw-loader并修改entry入口，将html文件写入,
*/
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  mode: 'development',
  target: 'web',
  entry: ['./src/index.js', './src/index.html'],
  output: {
    filename: 'built5.js',
    path: path.resolve(__dirname, 'built5')
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader' ]
      },
      {
        //要加入raw-loader配置html文件才能热更新
        test: /\.html$/i,
        loader: "raw-loader"
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/built.css'
    })
  ],
  devServer: {
    // 打包好的项目路径和上面output的path一致
    static: {
      directory: path.resolve(__dirname, 'built5')
    },
    // 是否利用gzips压缩
    compress: true,
    // 热重载
    hot: true,
    // 端口号
    port: 9000
    // 是否默认打开浏览器
    // open: true
  }
}
```

## source-map

在和mode同级的配置项中：添加配置项`devtool: 'eval-source-map'`,可以添加响应的source-map

**source-map:一种提供源代码到构建后代码映射技术**

 (如果构建后代码出错，可以通过映射关系找到相应的源代码错误)

  内联和外部区别：

1. 外部生产了文件，内联没有
2. 内联构建速度更快

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  //修改source-map 
  devtool: 'eval-source-map',
  //模式
  mode: 'development',
  //改成数组，写入多个文件，热更新
  entry: ['./js/index.js', './src/index.html'],
  //出口文件
  output: {
    filename: 'js/built.js',
    path: path.resolve(__dirname, 'built')
  },
  module: {
    rules: [
      {
        //处理css文件
        test: /\.css$/i,
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.less$/i,
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      {
        // 有个问题：默认处理不了html中引入的img图片
        // 处理图片资源
        test: /\.(jpg|png|gif)$/,
        // 当只有一个loader时，可以不用use
        // 使用url-loader需要file-loader
        loader: 'url-loader',
        options: {
          limit: 8* 1024,
          // esModule: false 配置项是关闭一个模块的es6模块化，使用commonJS去解析

          //[hash:10]取图片的hash的前10位
          //[ext]取文件本身的后缀名
          name: '[hash:10].[ext]',
          // 指定输出路径（是在，输出路径中的子目录）
          outputPath: 'images'
        }
      },
      {
        // 处理html中img元素
        test: /\.html$/i,
        loader: 'html-loader'
      },
      {
        // 打包其他资源
        // 排除掉我们处理过的资源，其他都打包
        exclude: /\.(css|js|html|less|png|jpg|gif)$/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          // 指定输出路径（是在，输出路径中的子目录）
          outputPath: 'media'
        }
      }
    ]
  },
  plugins: [
    //处理html文件的
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  devServer: {
    // 项目构建后的路径
    contentBase: path.resolve(__dirname, 'built'),
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 开启HMR功能，热模块替换
    hot: true
  }
}
```



---

`  [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map`

  source-map：外部

​    作用：可以提供错误代码的准确信息和源代码的错误位置

---

  inline-source-map: 内联

​    只生成一个内联的source-map

​    作用：可以提供错误代码的准确信息和源代码的错误位置

---

  hidden-source-map: 外部

​    作用：只能提供错误代码的错误原因，不能提供错误位置，只能提示构建后代码错误位置

---

  eval-source-map:内联

​    每一个文件都生成对应的source-map，都在eval中

​    作用：可以提供错误代码的准确信息和源代码的错误位置，多了一个hash值而已

---

  nosources-source-map：外部

​    作用：能看到错误代码的准确信息，但是没有源代码信息

---

  cheap-source-map：外部

​    作用:提供错误准确信息，但是只能精确到行

---

  cheap-module-source-map：外部

​    作用：提供错误代码准确信息和源代码错误位置

​    module：会将loader的source-map也加入调试

---

### 生产环境与开发环境source-map模式选择

  开发环境：速度快，调试更友好

  速度方面(eval>inline>cheap)

  调试友好方面(source-map>cheap-module-source-map>cheap-source-map)

  **综合选择：eval-source-map**(vue,react脚手架采取的方案)

eval-cheap-module-source-map：提示信息更完全

---

  生产环境：源代码是否需要隐藏？调试是否更友好？

  内联方式会让代码体积变大，所以生成模式用外部方式

  如果需要隐藏：

​    nosources-source-map 全部隐藏

​    hidden-source-map 隐藏源代码

  如果需要调试友好：

​    source-map

​    cheap-module-source-map

## loader加载优化(oneOf:[])

如果单纯的配置module中的rules，通常来说，每个文件都会将rules中的所有loader全走一遍，但是有些文件只需要一个loader，全走一遍就浪费了时间。

所以rules中的一个配置项：`oneOf: []`

说明：指oneOf中的loader只会匹配一个，可以提示构建速度

注意：**不能出现两个配置处理一个文件的情况，如果有，需要拉出多的配置放在外面，oneOf中处理一种文件的loader只能有一个**

```js
module: {
  rules: [
  	{
  	  //oneOf中多出的loader放在外面
  	},
    {
      oneOf: [
      	//loader...
      ]
    }
  ]
}
```

## 缓存优化

**webpack缓存优化的核心：**

保证 hash 值的唯一性，即为每个打包后的资源生成一个独一无二的 hash 值，只要打包内容不一致，那么 hash 值就不一致。
保证 hash 值的稳定性，我们需要做到修改某个模块的时候，只有受影响的打包后文件 hash 值改变，与该模块无关的打包文件 hash 值不变。

那么就有一个完美的hash值：contenthash

在文件名跟hash目的：就是让浏览器发现文件名不同了不会对不上自己曾经缓存的东西（没有命中强缓存），需要去请求服务器了

```js
/*
  缓存：
    babel缓存
      cacheDirectory: true
      -->让第二次打包速度更快

    文件资源缓存：在要打包的资源的文件名添加hash值
      1.hash：每次webpack构建时会生成唯一的hash值(文件变或者不变，hash值都会改变)
      问题：因为js和css同时使用一个hash值，如果重写打包，会导致所有缓存失效(但是我却只改动一个文件)

      2.chunkhash：根据chunk生成的hash值，如果打包来源同一个chunk，那么hash值就是一样(打包文件同属于一个入口文件，那么chunk就一样)
      问题：js和css的hash值任然一样，因为css是再js中引入的所以属于同一个chunk

      3.contenthash：根据文件内容生成hash值，不同文件的hash值一定不同
      [contenthash:10]添加到要打包好的文件名上
      -->让代码上线运行缓存更好使用
*/
const {resolve} = require('path');
// 将css独立初一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
// 压缩css
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
// 处理HTML文件
const HtmlWebpackPlugin = require('html-webpack-plugin');

// 定义nodejs环境变量: 决定使用那个环境的browserslist
process.env.NODE_ENV = "production";

// 复用loader,
const commonCssLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  {
    // 兼容css还需要指明package.json中的browserslist
    loader: "postcss-loader",
    options: {
      ident: 'postcss',
      plugins:() => [
        require('postcss-preset-env')()
      ]
    }
  }
]

module.exports = {
  devtool: 'source-map',
  mode: "production",
  entry: "./src/js/index.js",
  output: {
    //在输出文件名上添加[contenthash:10]
    filename: "js/built.[contenthash:10].js",
    path: resolve(__dirname, "build")
  },
  module: {
    rules: [
      // 将处理同一个文件的loader拉到oneOf之外
      {
        // 设置为最先执行的loader
        enforce: "pre",
        // 要在package.json中添加eslintConfig -->airbnb语法监测
        // 语法检查eslint
        test: /\.js$/i,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          // 自动修复语法问题
          fix: true
        }
      },
     {
       //以下loader只会处理一个
       oneOf: [
        {
          // 处理css
          test: /\.css$/i,
          use: [...commonCssLoader/*展开复用的loader*/]
        },
        {
          // 处理less
          test: /\.less$/i,
          use: [
            // 展开复用的loader
            ...commonCssLoader,
            // less-loader就是将less变为css文件
            'less-loader'
          ]
        },
        {
          // 对js做兼容性处理
          test: /\.js$/i,
          exclude: /node_modules/,
          loader: "babel-loader",
          options: {
            presets: [
              [
                "@babel/preset-env",
                {
                  useBuiltIns: "usage",
                  corejs: {version: 3},
                  targets: {
                    chrome: "60",
                    firefox: "60"
                  }
                }
              ]
            ],
            // 开启babel缓存
            // 第二次构建时，会读取之前的缓存
            cacheDirectory:true
          }
        },
        {
          // 处理图片
          test: /\.(jpg|png|gif)$/i,
          loader: "url-loader",
          options: {
            limit: 8* 1024,
            name: "[hash:10].[ext]",
            outputPath: "images",
            publicPath: "../images",
            
          }
        },
        {
          // 处理html中的img元素内图片
          test: /\.html$/i,
          loader: "html-loader"
        },
        {
          // 处理其他文件
          exclude: /\.(js|css|less|html|png|jpg|gif|json)$/i,
          loader: 'file-loader',
          options: {
            outputPath: 'media'
          }
        }
       ]
     }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      // 修改一下打包后css文件的路径添加[contenthash:10]
      filename: 'css/built.[contenthash:10].css'
    }),
    // 压缩css代码插件
    new OptimizeCssAssetsWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html',
      // 压缩html文件
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    })
  ]
}
```

## tree shaking（满足前提自动开启）

**前提：1.必须使用ES6模块化，2.webpack开启production模式**

目的：去除无用代码

作用：减少代码体积，

还要注意一个问题：

如果在package.json文件中有配置：

`"sideEffects": false` 所有代码都没有副作用(所有文件都可以tree shaking)

那么tree shaking可能会把css，或者其他有副作用的文件给摇没了

**解决方式：`"sideEffects": ["*.css",...]`标记一下就不会处理了，其他文件也能标记**



## code-split代码分隔(按需引入的基础)4，5通用

### 第一种方式：将入口文件写成多入口方式

`entry: {xxx: "入口一", xxx2: "入口二"...}`

这样每一个入口都会打包成一个chunk

```js

const {resolve} = require('path');
// 处理HTML文件
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports = {
  mode: "production",
  // 单入口方式：只有一个入口，最终只有一个bundle(单页面应用)
  // entry: "./src/js/index.js"
  // 多入口(多页面应用)
  entry: {
    // 一个入口就是一个bundle
    main: "./src/js/index.js",
    test: "./src/js/test.js"
  },
  output: {
    // [name]：取文件名 和上面main，test一致
    filename: "js/[name].[contenthash:10].js",
    path: resolve(__dirname, "build")
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      // 压缩html文件
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    })
  ]
}
```

### 第二种方式：单入口或多入口配合optimization

添加配置项：optimization

单，多入口都挺好用

​    1.(单，多入口)都可以将node_modules中代码单独打包成一个bundle最终输出

​    2.自动分析多入口chunk中，是否含有公共文件，如果有，单独打包成一个bundle(不会重复打包)

```js

const {resolve} = require('path');
// 处理HTML文件
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports = {
  mode: "production",
  // 单入口方式：只有一个入口，最终只有一个bundle(单页面应用)
  // entry: "./src/js/index.js",
  entry: {
    main: "./src/js/index.js",
    test: "./src/js/test.js"
  },
  output: {
    // [name]：取文件名 和上面main，test一致
    filename: "js/[name].[contenthash:10].js",
    path: resolve(__dirname, "build")
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      // 压缩html文件
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    })
  ],
  //optimization配置项
  optimization: {
    splitChunks: {
      chunks: 'all'
    },
    // 解决缓存失效问题
    // 解决修改a文件导致b文件contenhash改变的方法
    runtimeChunk: {
      name: entrypoint => `runtime-${entrypoint.name}`
    }
  }
}
```

### 第三种：单入口，在js代码中添加动态引入进而将代码分隔

如果想用**单入口**，又想将但入口中引入的一些文件打包成一个单独的chunk，这时候就需要动态引入了

在入口文件中，用动态引入方式引入文件

```js
/*
  通过js代码，让某个文件被单独打包成一个chunk
  import动态导入语法：能将某个文件单独打包
  /*webpackChunkName: "test"*/ 来设置名字
*/
import(/*webpackChunkName: "test"*/"./test")
.then(({mul, sum}) => {
  // eslint-disable-next-line
  console.log(mul(1, 2));

})
.catch(err => {
  console.log("文件加载失败");
})
```

---

webpack.config.js配置不变，和方式二的单入口，加optimization一样

```js

const {resolve} = require('path');
// 处理HTML文件
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports = {
  mode: "production",
  // 单入口方式：只有一个入口，最终只有一个bundle(单页面应用)
  entry: "./src/js/index.js",
  output: {
    // [name]：取文件名 和上面main，test一致
    filename: "js/[name].[contenthash:10].js",
    path: resolve(__dirname, "build")
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      // 压缩html文件
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    })
  ],
  //optimization配置项
  optimization: {
    splitChunks: {
      chunks: 'all'
    },
    // 解决缓存失效问题
    // 解决修改a文件导致b文件contenhash改变的方法
    runtimeChunk: {
      name: entrypoint => `runtime-${entrypoint.name}`
    }
  }
}
```

## js代码懒加载和预加载

懒加载的配置方式：利用了es10中的动态导入import，在js文件中添加，**当在一定的条件触发之后，再引入资源，这就是懒加载**

预加载Prefetch：正常加载时并行加载（同一时间加载多个文件），当文件数量过多时，就会导致卡顿等负面影响。**而预加载就是当浏览器空闲了之后，再偷偷加载预加载资源**（有兼容性问题）

预加载配置方式：在魔法注释中添加`webpackPrefetch: true`

```js
console.log("index.js文件被加载了~");

//当点击之后，再加载
document.getElementById("btn").onclick = function() {
  // 在一定条件下动态引入就是懒加载
  // webpackPrefetch: true==>预加载：会提前加载js文件
  // Prefetch预加载：会等其他资源加载完毕，浏览器空闲了，偷偷再去加载资源
  import(/*webpackChunkName: 'test', webpackPrefetch: true*/"./test").then(({mul}) => {
    console.log(mul(4, 5));
  })
}
```

## PWA渐进式网络应用程序

作用：就是会缓存一部分资源，当处于离线状态时，仍然能够使用一些功能

要求：

1)首先要下载workbox-webpack-plugin并在webpack配置文件中配置

2)在入口文件注册serviceWorker

```js
/*
  出现的问题：
    1. eslint不认识window，navigator等全局变量
      解决方式：在package.json中的eslintConfig添加
        "env": {
        "browser": true//支持浏览器的全局变量
        }
    2. sw必须运行在服务器上
      解决方式:
      1.nodejs
      2.安装一个serve(npm i serve -g)
        serve -s build 启动服务器，将build目录下的静态资源全部暴露出去
*/
// 注册serviceWorker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then(() => {
        console.log('sw注册成功');
      })
      .catch(() => {
        console.log('注册成功');
      });
  });
}
```

webpack的配置文件：

```js
/*
 PWA：渐进式网络开发应用程序(就是离线访问技术)
  workbox --> workbox-webpack-plugin
*/
const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

const {resolve} = require('path');
// 将css独立初一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
// 压缩css
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
// 处理HTML文件
const HtmlWebpackPlugin = require('html-webpack-plugin');

// 定义nodejs环境变量: 决定使用那个环境的browserslist
process.env.NODE_ENV = "production";

// 复用loader,
const commonCssLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  {
    // 兼容css还需要指明package.json中的browserslist
    loader: "postcss-loader",
    options: {
      ident: 'postcss',
      plugins:() => [
        require('postcss-preset-env')()
      ]
    }
  }
]

module.exports = {
  devtool: 'source-map',
  mode: "production",
  entry: "./src/js/index.js",
  output: {
    filename: "js/built.[contenthash:10].js",
    path: resolve(__dirname, "build")
  },
  module: {
    rules: [
      // 将处理同一个文件的loader拉到oneOf之外
      {
        // 设置为最先执行的loader
        enforce: "pre",
        // 要在package.json中添加eslintConfig -->airbnb语法监测
        // 语法检查eslint
        test: /\.js$/i,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          // 自动修复语法问题
          fix: true
        }
      },
     {
       //以下loader只会处理一个
       oneOf: [
        {
          // 处理css
          test: /\.css$/i,
          use: [...commonCssLoader/*展开复用的loader*/]
        },
        {
          // 处理less
          test: /\.less$/i,
          use: [
            // 展开复用的loader
            ...commonCssLoader,
            // less-loader就是将less变为css文件
            'less-loader'
          ]
        },
        {
          // 对js做兼容性处理
          test: /\.js$/i,
          exclude: /node_modules/,
          loader: "babel-loader",
          options: {
            presets: [
              [
                "@babel/preset-env",
                {
                  useBuiltIns: "usage",
                  corejs: {version: 3},
                  targets: {
                    chrome: "60",
                    firefox: "60"
                  }
                }
              ]
            ],
            // 开启babel缓存
            // 第二次构建时，会读取之前的缓存
            cacheDirectory:true
          }
        },
        {
          // 处理图片
          test: /\.(jpg|png|gif)$/i,
          loader: "url-loader",
          options: {
            limit: 8* 1024,
            name: "[hash:10].[ext]",
            outputPath: "images",
            publicPath: "../images",
            
          }
        },
        {
          // 处理html中的img元素内图片
          test: /\.html$/i,
          loader: "html-loader"
        },
        {
          // 处理其他文件
          exclude: /\.(js|css|less|html|png|jpg|gif|json)$/i,
          loader: 'file-loader',
          options: {
            outputPath: 'media'
          }
        }
       ]
     }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      // 修改一下打包后css文件的路径
      filename: 'css/built.[contenthash:10].css'
    }),
    // 压缩css代码插件
    new OptimizeCssAssetsWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html',
      // 压缩html文件
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    }),
    // 添加pwa要使用的插件并配置
    new WorkboxWebpackPlugin.GenerateSW({
      /*
        1.帮助serviceworker快速启动
        2.删除旧的 serviceworker

        生成一个 serviceworker配置文件
      */
      clientsClaim: true,
      skipWaiting: true
    })
  ]
}
```

## 多进程打包

需要：thread-loader

开启多进程打包

进程启动大概需要600ms，进程通信也有开销

**只有工作时间较长的，才需要开启多进程打包**（一般都针对babel loader，because babel-loader非常慢）

```js
{
    loader: 'thread-loader',
    options: {
    workers: 2//开启两个进程
    }
}
```

## externals 排除打包（使用cdn引入）

**当一些包需要用cdn引入**，就不用打包了，就可以使用externals配置项配置一下：

```js
//和mode，entry...同级配置
externals: {
    // 忽略库名 -- npm包名
    // 拒绝jquery这个包被打包进来
    jquery: 'jQuery'
  }
```



## Dll技术

dll技术作用：先将要打包的资源全部先打包在一起，如果想要用就从这个资源中引入就好，简化了一点点操作。

步骤：

1、首先要配置并运行一个webpack.dll.js文件(不一定非要叫dll)

运行指令：webpack --config webpack.dll.js

```js
/*
  使用dll技术，对某些库（第三方库：jq，react...）进行单独打包
    当你运行webpack时，默认查找的时webpack.config.js配置
    如果想要运行webpack.dll.js文件
    -->指令：webpack --config webpack.dll.js
*/
const {resolve} = require('path');
const webpack = require('webpack');
module.exports = {
  mode: 'production',
  entry: {
    // 最终打包生成的[name] ---> jquery
    // 可以写多个
    jquery: ['jquery']
    // react: ['react', 'react-dom',...]
  },
  output: {
    filename:'[name].js',
    path: resolve(__dirname, 'dll'),
    library: '[name]_[hash]'//打包的库里面向外暴露出去的内容叫什么名字
  },
  plugins: [
    // 打包生成一个manifest.json -->提供和jquery的映射
    new webpack.DllPlugin({
      name: '[name]_[hash]',//映射库的暴露的内容名称
      path: resolve(__dirname, 'dll/manifest.json')
    })
  ]
}
```

2、然后配置webpack.config.js

```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
// dll技术需要的俩包
const webpack = require('webpack');
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build')
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    // 告诉webpack那些库不需要参加打包，同时使用时的名称也需要改变
    new webpack.DllReferencePlugin({
      //和dll文件中打包出来的manifest.json文件对应
      manifest: resolve(__dirname, 'dll/manifest.json')
    }),
    // 将某个文件打包出去，并在html中自动引入改资源
    new AddAssetHtmlWebpackPlugin({
      //引入刚刚打包在一起的资源  
      filepath: resolve(__dirname, 'dll/jquery.js')
    })
  ],
  mode: 'production'
}
```

