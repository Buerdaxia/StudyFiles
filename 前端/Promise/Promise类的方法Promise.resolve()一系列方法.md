# Promise.resolve()一系列方法

# Promise的方法

## 1 Promise.resolve 方法

Promise.resolve(value)；

1. value：是成功的数据或者promise对象

说明：**有时候需要将现有的对象转换成Promise对象，该方法就起作用了**

```js
Promise.resolve('foo');
//等价于 下面写法

new Promise(resolve => resolve('foo'));
```

`Promise.resolve()`方法的参数分成四种情况。

1. **参数是一个 Promise 实例**

如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例

2. **参数是一个`thenable`对象**

`thenable`对象指的是具有`then`方法的对象，比如下面这个对象。

```js
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
```

Promise.resolve()会将这个对象转换为Promise对象，然后立即执行`thenable`对象的`then`方法。

```js
    let thenable = {
      then: function(resolve) {
        resolve(10);
      }
    }
    let p1 = Promise.resolve(thenable);

    p1.then(res => {
      console.log(res);//10
    })
//上面代码中，thenable对象的then()方法执行后，对象p1的状态就变为resolved，从而立即执行最后那个then()方法指定的回调函数，输出10。
```



3. **参数不是具有`then()`方法的对象，或根本就不是对象**

如果参数是一个原始值，或者是一个不具有`then()`方法的对象，则`Promise.resolve()`方法返回一个新的 Promise 对象，状态为`resolved`成功的。

```js
let p2 = Promise.resolve(521);
    p2.then(res => {
      console.log('p2res:', res);
    })
```

4. **不带有任何参数**

`Promise.resolve()`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。

所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用`Promise.resolve()`方法。

```js
const p = Promise.resolve();

p.then(function () {
  // ...
});
```

需要注意的是，立即resolve()的promise对象，是在本轮事件循环的结束时执行，而不是在下一列事件循环的开始时执行。

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```



## 2 Promise.reject 方法

Promise.resolve(reason)；

1. reason：失败的原因

说明：返回一个失败的promise对象(无论你传递啥都是失败的)

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

`Promise.reject()`方法的参数，会原封不动地作为`reject`的理由，变成后续方法的参数。

```js
Promise.reject('出错了')
.catch(e => {
  console.log(e === '出错了')
})
// true
```



## 3 Promise.all 方法

Promise.all(promises);返回的还是一个Promise的对象

1. promises：包含n个promise的数组

```js
const p = Promise.all([p1, p2, p3]);
```

`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

```js
Promise.all([p1, p2, p3]).then(res=> {
  console.log(res);
  //res是一个数组，是p1，p2，p3各自成功的返回值
});
```



（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

代码举例：(nodejs)**读取文件并写入**

```js
const fs = require('fs');
const path = require('path');
const util = require('util');

let filePath1 = path.join(__dirname, 'files', 'my.txt');
let filePath2 = path.join(__dirname, 'files', 'love.txt');
let filePath3 = path.join(__dirname, 'files', 'nodejs.txt');
let filePath4 = path.join(__dirname, 'files', 'data.txt');

let readFilePromise = util.promisify(fs.readFile);
let writeFilePromise = util.promisify(fs.writeFile);

let p1 = readFilePromise(filePath1, 'utf-8');
let p2 = readFilePromise(filePath2, 'utf-8');
let p3 = readFilePromise(filePath3, 'utf-8');
/* Promise.all([Promise对象1, Promise对象2, Promise对象3...])
   .then(data=> {
     data各个Promise对象成功的返回值所组成的数组
   })
*/
let str = '';
Promise.all([p1, p2, p3])
	.then(data => {
		console.log(data); // ['我']
		str = data.join('');
		writeFilePromise(filePath4, str); //写入到filePath4中
		console.log(str); // 我爱node.js
	})
	.catch(err => {
		// 从p1,p2,p3中捕获第一个变为rejected的对象
		console.log(err);
	});

```



## 4 Promise.race 方法

Promise.race(promises);

1. promises：包含n个promise对象的数组

```js
const p = Promise.race([p1, p2, p3]);
```

说明：上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

例子：

```js
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    // 5s之后变为失败状态
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
// 如果fetch在5s之内还没有结果(成功或者失败),那么就返回的是第二个promise失败的状态
```

race与all方法的区别：

all是全部成功，则成功并且返回一个成功的值组成的数组，否则失败

race是谁先成功，则返回那个率先成功的值

相同点：**都是传递一个由`Promise`对象组成的数组**

## 5 Promise.any()方法

该方法也接收一组Promise实例作为参数，包装成一个新的Promise实例返回。

```js
Promise.any([
  fetch('https://v8.dev/').then(() => 'home'),
  fetch('https://v8.dev/blog').then(() => 'blog'),
  fetch('https://v8.dev/docs').then(() => 'docs')
]).then((first) => {  // 只要有一个 fetch() 请求成功
  console.log(first);
}).catch((error) => { // 所有三个 fetch() 全部请求失败
  console.log(error);
});
```

说明：该函数的参数中，只要有一个参数实例变为`fulfilled`状态(成功状态)，那么包装的实例就会变为`fulfilled`状态，如果所有参数实例都为`rejected`，则包装的实例就会变为`rejected`状态。

`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是`Promise.any()`不会因为某个 Promise 变成`rejected`状态而结束，必须等到所有参数 Promise 变成`rejected`状态才会结束。

