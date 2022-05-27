# JS中的三类循环对比及性能分析

## 稀疏数组和密集数组

稀疏数组：指数组内容并不是填满的，可能不连续，切有空位

例如：

```js
let arr = [1, ,2,3,4];
let arr2 = new Array(10);// 创建一个长度为10的数组
```



密集数组：指数组内容连续填充，并且填满。

例如：

```js
let arr = [1, 2, 4, 5];
let arr2 = new Array(10).fill(0);//长度为10，每项都是0；
```





## console.time(xx)与console.timeEnd(xx);

这两个`console`的方法可以让我们来测试程序走了多少毫秒，必须要成对出现

语法：

```js
console.time('time');
xxxxx
console.timeEnd('time');

// 控制台将输出time: xxxx.xxxms
```



## for循环与while循环与forEach循环

性能比较：

1. 当`for`循环和`while`循环，基于`var`声明变量时，性能是差不多的（不确定循环次数下使用`while`）

```js
    let arr = new Array(9999999).fill(0);

    console.time('For~~');
    for (var i = 0; i < arr.length; i++) { }
    console.timeEnd('For~~');

    console.time('While~~');
    var i = 0;
    while (i < arr.length) {
      i++;
    }
    console.timeEnd('While~~');

/*
	输出：
	For~~: 18.457275390625 ms
	While~~: 18.375 ms
*/
```

2. 当基于`let`声明时，`for`循环的性能更加优秀。原因：**没有创造全局不释放的变量**

```js
    let arr = new Array(9999999).fill(0);

    console.time('For~~');
		// 创建了局部变量i，使用完毕就释放了，所以性能占用更少所以更快
    for (let i = 0; i < arr.length; i++) { }
    console.timeEnd('For~~');

    console.time('While~~');
		// 创建了全局变量i，一致消耗性能
    let i = 0;
    while (i < arr.length) {
      i++;
    }
    console.timeEnd('While~~');
/*
	输出：
	For~~: 6.269287109375 ms
  While~~: 19.9658203125 ms
*/
```

3. `for`和`while`在底层性能上差不多，唯一差距就是全局变量与局部变量的问题，但是`forEach`相比于他们俩性能是最差的。

```js
    let arr = new Array(9999999).fill(0);

    console.time('For~~');
    for (let i = 0; i < arr.length; i++) { }
    console.timeEnd('For~~');

    console.time('While~~');
    let i = 0;
    while (i < arr.length) {
      i++;
    }
    console.timeEnd('While~~');

    console.time('ForEach~~');
    arr.forEach(function (item) { });
    console.timeEnd('ForEach~~');
/*
结果：
For~~: 6.402099609375 ms
While~~: 19.97900390625 ms
ForEach~~: 75.47998046875 ms
*/
```



## 重写forEach循环

我们自己在`Array`的原型上重写forEach覆盖一下，自己重写

代码：

```js
    Array.prototype.forEach = function forEach(callback, context) {
      // this -> arr(调用实例);
      let self = this;
      i = 0;
      len = this.length;
      // 判断context是否传递值
      context = context == null ? window : context;
      for (; i < len; i++) {
        // 判断是否为function，是就调用并将数组每一项传入
        typeof callback === 'function' ? callback.call(context, self[i]) : null;
      }
    }
```



## for...in循环（速度最慢）

for...in的性能很差，很差的原因：迭代的当前对象中所有可枚举的属性（私有属性大部分可枚举，公有属性部分可枚举），`for...in`的查找机制（**遍历当前所有可枚举属性**）导致它会沿着原型链上进行查找，自然就很慢了

```js
    let arr = new Array(9999999).fill(0);

    console.time('For~~');
    for (let i = 0; i < arr.length; i++) { }
    console.timeEnd('For~~');

    console.time('While~~');
    let i = 0;
    while (i < arr.length) {
      i++;
    }
    console.timeEnd('While~~');

    console.time('ForEach~~');
    arr.forEach(function (item) { });
    console.timeEnd('ForEach~~');

		console.time('For in~~');
    for (let key in arr) { }
    console.timeEnd('For in~~');
/*
结果：
For~~: 6.402099609375 ms
While~~: 19.97900390625 ms
ForEach~~: 75.47998046875 ms
For in~~: 1990.171142578125 ms//最慢
*/
```



当然如果非要用`for...in`方法，还要面临这三个问题：

1. 遍历顺序以数字优先（无法解决）
2. 会遍历公有的可枚举属性（会沿着原型链遍历）=>解决

```js
// 问题2解决方式：
    let obj = {
      name: 'qec',
      a: 100,
      b: 299,
      [Symbol('AA')]: 100,
      0: 200
    }
    for (let key in obj) {
      // 利用hasOwnProperty方法即可，该方法不会查找原型链
      if (!obj.hasOwnProperty(key)) break;
      console.log(key);
    }
```

3. 无法遍历`Symbol`属性（可解决）

```js
// 问题3解决方式
    let obj = {
      name: 'qec',
      a: 100,
      b: 299,
      [Symbol('AA')]: 100,
      0: 200
    }
    // 第一步：利用Object.keys()方法，读取所有非[Symbol]属性名
    let keys = Object.keys(obj);
    // 第二步：利用concat和Object.getOwnPropertySymbols()方法获取[Symbol]属性名
    if (typeof Symbol !== 'undefined') keys = keys.concat(Object.getOwnPropertySymbols(obj));
		// 遍历
    keys.forEach(key => {
      console.log('属性名', key);
      console.log('属性值', obj[key]);
    })
```



## for...of循环

部分数据结构实现了迭代器规范。(**强调没有对象！！！**)，如果一个数据结构身上有[Symbol.iterator]属性，我们就说改数据结构实现了迭代器规范。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

`for...of`原理：就是按照迭代器规范进行遍历的。

迭代器自我封装的方法：（for...of的内部机制）

```js
    let arr2 = [10, 20, 30, 40];
    arr2[Symbol.iterator] = function () {
      let self = this;
      index = 0;
      return {
        // 返回的对象必须具备一个next()方法
        /*
          done: 指是否停止
          value: 每一次获取的值
        */
        next() {
          return index < self.length ?
            {
              value: self[index++],
              done: false
            }
            : {
              value: undefined,
              done: true
            }
        }
      }
    }

    for (let key of arr2) {
      console.log(key);
    }
/*
10
20
30
40
*/
```



由于对象默认是没有`[Symbol.iterator]`属性的，所以不能使用`for...of`方法，但是我们可以自己给对象上加生成器！

改造对象(伪数组):(拥有length属性，切属性值是连续的)

```js
    let obj = {
      0: 10,
      1: 20,
      2: 30,
      length: 3
    }
    // 直接添加一个迭代器，和数组的一样的迭代器（或者自己写一个也行0，0）
    obj[Symbol.iterator] = Array.prototype[Symbol.iterator];
		
		// 添加完毕后对象也可以使用for...of方法
    for (let item of obj) {
      console.log(item);
    }
/*
10,
20,
30
*/
```

