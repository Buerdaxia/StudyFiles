# jQuery中的静态方法

首先，JS中的静态方法，就是添加给类的方法对吧，ES5是添加到构造函数上，ES6是通过static直接添加到类上

核心：**记住静态方法是由构造函数(类名儿)直接调用的**

示例：

```js
// ES5
function Aclass() {
	// 构造函数
}

// 添加一个静态方法
Aclass.staticMethod = function () {
  alert('我是静态方法');
}

// 添加一个实例方法
Aclass.prototype.entityMethod = function () {
  alert('我是实例方法');
}

// 调用静态方法
Aclass.staticMethod();

// 调用实例方法
new Aclass().entityMethod();
```





## 1：`each`

这个方法类似于原生JS中的`forEach`方法，but！each方法更加强大（既能遍伪数组，也能遍历数组）。

注意点：**`forEach`和`each`中，接收的回调函数参数接收顺序不同**

语法：

```js
// 注意第一个是索引index，第二个是value值
$.each(arr, function(index, value) {
  //功能
})
```





## 2：`map`

该函数和原生map函数也类似(其实功能是一样的)，也是原生map的加强版！

注意：`$.map`是可以遍历伪数组的呦，原生不行

语法：

```js
// 注意：回调函数的接受值，第一个是value，第二个是索引
$.map(obj, function (value, index) {
  console.log(index, value);
})
```



## each和map的区别

首先，他俩都能遍历数组，也都能遍历伪数组，那么他俩究竟有啥区别呢？

其实他俩的区别：就像原生中`forEach`和`map`的区别一样，功能上的区别。

区别：

1. `each`默认返回遍历原数组，`map`如果在回调没有return的时候，默认返回空数组
2. `each`不支持修改返回的数组，`map`支持修改，并是重新创建的一个数组



所以说：功能和原生一样，如果只是单纯遍历，使用`$.each()`，如果要遍历的同时修改数组并返回新值，请使用`$.map()`





## 3：`trim`

和原生trim方法一样，去除字符串左右两边多余的空格

语法：

```js
$.trim(字符串);
```

返回值：一个新的字符串(去除过空格的)

```js
let str = '   a   b   c   ';
console.log(str);
console.log(str.length); // 15

let str2 = $.trim(str);
console.log(str2);
console.log(str2.length); // 9
```



## 4：`isWindow`

判断传入的对象是否是window顶级对象

语法：

```js
$.isWindow(对象);
```

返回值：Boolean



## 5：`isArray`

判断传入的是否是一个数组

语法：

```js
$.isArray(参数);
```

返回值：Boolean



## 6：`isFunction`

判断是否是一个函数

语法：

```js
$.isFunction(参数);
```

返回值：Boolean



## 7：`holdReady`

暂停入口函数的执行，就是有些场景，必须想让dom全部加载完毕，或者一些其他的事情做完，才执行ready事件时，就可以使用该方法。

作用：能够控制JQ代码块的执行

语法：

```js
$.holdReady(true); //暂停ready方法

$.holdReady(false); // 启动
```



示例：

```html
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>jQuery静态方法holdReady</title>
  <script src="../js/jquery-3.6.0.js"></script>
  <script>
    // 暂停：
    $.holdReady(true);
    $(function () {
      alert('111');
    })
  </script>
</head>

<body>
  <button>点我重新执行</button>
</body>
<script>

  const btn = document.getElementsByTagName('button')[0];

  btn.addEventListener('click', function () {
    // 点击按钮，开启ready事件
    $.holdReady(false);
  })
</script>
```



## 其余的去官网查询

