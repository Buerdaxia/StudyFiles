## CommonJS的Modules规范：

一个js文件即使一个模块，模块里的内容想给别人用，那就需要暴露出去，别人就需要引入进来。

这种规范在nodeJs中使用的

commonjs中一个模块的内容是私有的，每一个模块都有自己的作用域。只能通过暴露手段向外导出。



导出：实际上就是一个对象去添加属性，然后将这个对象暴露出去。

### 方式一：

导入：require关键字

```nodejs
const m1 = require("./modules/m1.js");

//m1是一个对象
//使用
m1.xxx
m1.xxx
m1.sum() {
 xxx
}
```

导出：export.xxx = 变量

把exports看作是一个对象，给他添加属性

```nodejs
//将num暴露出去,前面名称自定义，后面为要到处的数据
// 方式一：
exports.num = num;
exports.sum = sum;
exports.Animal = Animal;
```

### 方式二：用的比较多

导入一致，导出不同

第二种导出方式：

```js
module.exports = sum;//导出一个
// 方式二：导出多个
module.exports = {
//这里面用到是简写属性名
    num,
    sum,
    Animal
}
//防止exports跟丢了 如果使用第二个又需要exports就写以下代码
exports = module.exports;
```

在js文件模块中，`this`是指向这个模块导出的对象`module.exports`

交互模式下`this`指向`global`

exports和module.exports关系？

**exports是module.exports的引用，只有文件中才有exports，交互模式下没有exports**



# nodejs常用的内置模块

1）fs：文件操作

2）http：网络操作

3）path：路径操作

4）querystring：查询参数解析(解析get传来的params或者？后的参数)

5）url：url解析

```js
const fs = require("fs");
const http = require("http");
const path = require("path");
const querystring = require("querystring");
const url = require("url");
```

