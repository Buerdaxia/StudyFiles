# Promise实例对象的方法

## 一、Promise.prototype.then方法

Promise.prototype.then((onResolved, onRejected)=>{});

1. onResolved函数：成功的回调函数(value) => {}
2. onRejected函数：失败的回调函数(reason) => {}

注意：指定用于得到value的成功回调和用于得到失败reason的失败回调，**返回一个新的promise对象**，所以才能够进行链式写法

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
  //返回的json.post 会作为下面这个then方法的参数
}).then(function(post) {
  // ...
});
```

采用链式的`then`，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个`Promise`对象（即有异步操作），这时后一个回调函数，就会等待该`Promise`对象的状态发生变化，才会被调用。

## 二、Promise.prototype.catch()方法

Promise.prototype.catch((onRejected)=>{})

1. onRejected函数：失败的回调函数(reason) => {}

注意：**该方法只能得失败的结果**，也就是reject的回调，

```js
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

注意：`catch`也能捕获`then()`方法回调抛出的错误



```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });
// ok 不会走catch
```

还有，Promise在resolve语句之后抛出错误，就不会被捕获，应为promise状态一旦改变不能再变了。



Promise对象的错误有冒泡的性质，会一直向后转递，直到被捕获位置。也就是说，错误总是会被下一个`catch`语句捕获，也就是异常的穿透。

## 三、Promise.prototype.finally()方法

`finally()`方法用于指定不管Promise对象最后状态如何，都会执行的操作。

返回值：返回一个新的期约(`promise`)实例

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```



注意：**这个新期约实例不同于 then()或 catch()方式返回的实例。因为 onFinally 被设计为一个状态 无关的方法，所以在大多数情况下它将表现为父期约的传递。对于已解决状态和被拒绝状态都是如此。**



## 期约连锁（promise的链式调用）

因为实例方法（`then`、`catch`、`finally`）都会返回一个**新的**期约对象，而这个期约对象又有自己的实例方法，所以就可以进行期约连锁。

示例：

```js
let p = new Promise((resolve, reject) => { 
 console.log('first'); 
 resolve(); 
}); 
p.then(() => console.log('second')) 
 .then(() => console.log('third')) 
 .then(() => console.log('fourth')); 
// first 
// second 
// third 
// fourth
```



又或者这样：执行异步任务，**串行化**异步任务：

```js
let p1 = new Promise((resolve, reject) => { 
 console.log('p1 executor'); 
 setTimeout(resolve, 1000); 
}); 
p1.then(() => new Promise((resolve, reject) => { 
 console.log('p2 executor'); 
 setTimeout(resolve, 1000); 
 })) 
 .then(() => new Promise((resolve, reject) => { 
 console.log('p3 executor'); 
 setTimeout(resolve, 1000); 
 })) 
 .then(() => new Promise((resolve, reject) => { 
 console.log('p4 executor'); 
 setTimeout(resolve, 1000); 
 })); 
// p1 executor（1 秒后）
// p2 executor（2 秒后）
// p3 executor（3 秒后）
// p4 executor（4 秒后）
```



可以说：**期约连锁**就是解决”回调地狱“的核心关键

示例：



```js
// 期约连锁版本
function delayedResolve(str) { 
 return new Promise((resolve, reject) => { 
 console.log(str); 
 setTimeout(resolve, 1000); 
 }); 
} 
delayedResolve('p1 executor') 
 .then(() => delayedResolve('p2 executor')) 
 .then(() => delayedResolve('p3 executor')) 
 .then(() => delayedResolve('p4 executor')) 
// p1 executor（1 秒后）
// p2 executor（2 秒后）
// p3 executor（3 秒后）
// p4 executor（4 秒后）

// 回调函数版本
function delayedExecute(str, callback = null) { 
 setTimeout(() => { 
 console.log(str); 
 callback && callback(); 
 }, 1000) 
} 
delayedExecute('p1 callback', () => { 
 delayedExecute('p2 callback', () => { 
 delayedExecute('p3 callback', () => { 
 delayedExecute('p4 callback'); 
 }); 
 }); 
}); 
// p1 callback（1 秒后）
// p2 callback（2 秒后）
// p3 callback（3 秒后）
// p4 callback（4 秒后）

```

