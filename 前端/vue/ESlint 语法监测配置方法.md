# ESlint 语法监测配置方法

在vue项目中的.eslintrc.js文件中

rules中的规则如下

```js
  'rules': {
    // allow paren-less arrow functions
    'arrow-parens': 0,
    // allow async-await
    'generator-star-spacing': 0,
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0,
    "no-unused-vars": [2, { 
      // 允许声明未使用变量
      "vars": "local",
      // 参数不检查
      "args": "none" 
    }],
    // 关闭语句强制分号结尾
    "semi": [0],
    //空行最多不能超过100行
    "no-multiple-empty-lines": [0, {"max": 100}],
    //关闭禁止混用tab和空格
    "no-mixed-spaces-and-tabs": [0],
  }
```

## 如果语法检查遇到以下错误

Expected linebreaks to be 'LF' but found 'CRLF'  linebreak-style

.eslintrc文件 rules 里面 配置 "linebreak-style": [0 ,"error", "windows"], //允许windows开发环境即可

(自己开发的时候可以用)
