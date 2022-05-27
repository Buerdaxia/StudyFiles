# JavaScript数据类型检测

## typeof()方法

作用：检测数据类型(基本数据类型比较准确)

语法：

```js
typeof xx;
```

返回值：是一个字符串

示例：

```js
// 基础类型
typeof 12; // 'number'
typeof true; // 'boolean'
typeof 'str'; // 'string'
typeof undefined; // 'undefined'
typeof null; // 'object' 很特殊

// 引用类型
typeof {}; // 'object'
typeof []; // 'object'
typeof function(){}; // 'function'
...
```

底层机制：**直接在计算机底层基于数据类型的值(二进制)进行检测。**

注意点：`typeof null`为啥是一个对象`object`？

因为JavaScript中对象存储在计算机中时，是以000(二进制的)开始的二进制存储，而`null`的二进制标识是`000`所以会认为是一个对象类型

可以说是当初设计的缺陷。

缺点：

1. `typeof 普通对象/数组对象/正则对象/时间对象...`检测出来的都是`object`，所以说检测基础数据类型很好用（除了null）



## instanceof 方法

`instanceof`最初设计的并不是用来检测数据类型的，为了弥补一下`typeof`的缺陷，采用来检测数据类型。

作用：**检测当前实例是否属于这个类**

语法：

```js
let arr = [];
arr instanceof Array; // true
arr instanceof RegExp; // false
```

返回值：布尔值

示例：

```js
let arr = [];
arr instanceof Array; // true
arr instanceof RegExp; // false
arr instanceof Object; // true 特殊点

// 基本类型
1 instanceof Number; // false
```

底层机制：**只要当前类出现在实例的原型链上，结果都是`true`**

缺点：

1. 由于JavaScript允许我们自己重构原型链的指向，所以说要是我们自己修改了原型链，`instanceof`方法仍然不够准确
2. 致命缺陷，无法检测基本数据类型



自己分装instanceof方法

```js
/*
  核心：实例.__proto__ === 类.prototype
  Object.getPrototypeOf(xx)
*/
function instance_of(example, classFunc) {
  let classFuncPrototype = classFunc.prototype;
  let proto = Object.getPrototypeOf(example);

  while (true) {

    // 如果 实例.__proto__ === 类.prototype 返回true
    if (proto === classFuncPrototype) {
      return true;
    }
    // 如果以及查到原型链头了还没相等，就返回false
    if (proto === null) {
      return false;
    }
    // proto 等于下一个proto
    proto = Object.getPrototypeOf(proto);
  }
}
let arr = [];
let obj = {};
console.log(instance_of(arr, Array));// true
console.log(instance_of(arr, RegExp));// false
console.log(instance_of(arr, Object));// true
console.log(instance_of(obj, Object));// true
```



## constructor 方法

利用查看构造函数来判断是否是某个类型。

语法：

```js
let arr = [];
console.log(arr.constructor === Array);
console.log(arr.constructor === RegExp);
console.log(arr.constructor === Object);
```

示例：

```js
let arr = [];
let n = 1;
console.log(arr.constructor === Array); //true
console.log(n.constructor === Number); //true
```



缺点：

1. 由于constructor也是可以进行修改的，所以导致该方法检测也不准确。

例如：

```js
let n = 1;
Number.prototype.constructor = 'xx';
console.log(n.constructor === Number); // false
```



## Object.prototype.toString.call() 方法 标准检测方法

**该方法检测数据类型的标准方法。**

核心：`Object.prototype.toString()`方法是用来返回当前实例所属类的信息，之后再通过call()方法来改变调用的`this`，让this指向传进来的值不就ok了？

自己分装的封装：

```js
function type_of(value) {
    return Object.prototype.toString.call(value).slice(8, -1);
}
let arr = [];
let obj = {};
console.log(type_of(1)); // Number
console.log(type_of('s')); // String
console.log(type_of(arr)); // Array
console.log(type_of(obj)); // Object
console.log(type_of(function () { })); //Function

// 挂载到window直接使用

(function() {
function type_of(value) {
    return Object.prototype.toString.call(value).slice(8, -1);
}
// 挂载到window全局使用
window.type_of = type_of;
})
```



`jQuery`封装：

```js
(function () {
  // jQuery封装版
  var class2type = {};
  var toString = class2type.toString; // 等同于Object.prototype.toString

  // 设置数据类型映射表
  ['Boolean', 'Number', 'String', 'Function', 'Array', 'Date', 'RegExp', 'Object', 'Symbol', 'Error'].forEach((item) => {
    class2type[`[object ${item}]`] = item.toLowerCase();
  })

  // 创建检测函数
  function toType(obj) {
    if (obj == null) {
      // 判断null和undefined
      return obj + '';
    }
    // 如果是引用数据类型就判断，否则是基础类型用typeof
    return typeof obj === 'object' || typeof obj === 'function' ?
      class2type[toString.call(obj)] || 'object' :
      typeof obj;
  }
  window.toType = toType;
})()
console.log(toType(1));
console.log(toType('string'));
console.log(toType({}));
console.log(toType([]));
```

