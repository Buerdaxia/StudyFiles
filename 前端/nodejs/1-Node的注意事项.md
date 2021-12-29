# Node的注意事项

1 node.js： ECMAScript + 核心的api(重点)，没有DOM,BOM

JavaScript三大组成部分： ECMAScript + DOM + BOM



node.js一般提供了文件操作系统（fs），和web服务的功能（http）



注意：es6语法不支持低版本node.js 4，5不支持



## 全局对象

JavaScript有一个特殊的对象，成为全局对象(Global Object)，它及其所有属性都可以在程序的任何地方访问，即全局变量。

**在浏览器JavaScript中，通常windows是全局对象，**

**而在node.js中全局对象是global**



### 全局对象的问题：

1、nodejs里没有window对象，有的是global对象，之前使用过的console，setTimeou等

都是global下面的属性

```nodejs
console.log(global);
console.log(window);//报错
```



2、nodejs中声明的变量，不会挂载到global中

```nodejs
var a = 30;
console.log(global.a);//undefined

//在浏览器端，就会挂在到window上
var b = 30;
console.log(window.b);//30
```



3、**可以向global添加成员，可以在任何地方去使用**（定义全局变量）

**这个任意地方甚至没有暴露引入的不同模块的都可以**

```js
global.a = 60;//这个a可以在其他模块中直接使用
console.log(a);//60
```

4、在nodejs的一个执行文件中，里面的this指向谁？

**this，在文件中其实指向的是这个模块导出的对象（这个js文件）**

```
console.log(this === global);//false
console.log(this);
```

面试题：

**在交互模式下，this === global 是true的**

**在文件模块中，this指向这个模块导出的对象 module.exports**



process对象（仅做了解）

```js
console.log(process.argv);
//返回一个数组，前两个值是node命令所在位置，也可以获得你命令行输入的命令一同丢尽数组里

console.log(process.arch);//显示系统版本位数
```

## nodejs中新加入的Buffer数据类型

JavaScript语言本身只有字符串数据类型，并没有二进制数据类型。但是在处理文件流时（文件读写操作），必须使用到二进制数据。因此在Node.js中定义了一个Buffer类，该类用来创建一个专门存放二进制数据的缓存区。说白了Buffer类似于一个整数数组。

