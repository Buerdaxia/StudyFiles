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

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

