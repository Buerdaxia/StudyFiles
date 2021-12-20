# path内置模块

首先的俩变量

```js
// 得到当前执行的文件的绝对路径，但不包括文件名
console.log(__dirname);
// 得到当前执行的文件的绝对路径，包括文件名
console.log(__filename);
```

第一步：引入

`const path = require("path");`

基础操作

```js
const path = require("path");

// 得到当前执行的文件的绝对路径，但不包括文件名
console.log(__dirname);
// 得到当前执行的文件的绝对路径，包括文件名
console.log(__filename);


// 获取文件扩展名（后缀名）
console.log(path.extname(__filename));

// 获取文件名（包含后缀名）
console.log(path.basename(__filename));

// 获取指定文件当前所在的路径
console.log(path.dirname(__filename));

// 获取一个对象，对象包含文件的相关信息(文件名，路径，后缀名等..)
let parseObj = path.parse(__filename);
console.log(parseObj);

```



重要操作：

```js
// 拼接操作
// let fullPath = path.join('path.js');
// 可以拿到指定文件的完整路径
// 获取'01-模块化的使用.js完整路径
let fullPath = path.join(__dirname,'01-模块化的使用.js');
console.log(fullPath);
// 拿到m1的完整路径（跨文件夹）,一层目录就是一个参数
// m1 是在modules文件夹下
let fullPath = path.join(__dirname,'modules','m1.js');
console.log(fullPath);
```

