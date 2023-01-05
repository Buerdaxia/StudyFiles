# async与await

目的：ES8 的 async/await 旨在解决利用异步结构组织代码的问题

## async

async 关键字用于声明异步函数。这个关键字可以用在函数声明、函数表达式、箭头函数和方法上：

示例：

```js
// 普通
async function foo() {} 
// 字面量
let bar = async function() {}; 
// 箭头
let baz = async () => {}; 
class Qux { 
 async qux() {} 
} 
```



1. async函数的返回值是一个promise对象
2. promise对象的结果由async函数执行的返回值决定（就是由return语句决定）
3. 如果async函数状态没有改变，则返回一个`pending`状态`Promise`对象为（例如函数中有`await`，因为`await`会阻塞代码哟）

```js
async function fn() {
      // 
      return '嘻嘻嘻'
}

```

三种情况：
1：如果返回的不是一个promise对象

则async函数返回的是一个成功的`promise`对象，成功的值是这一个**返回的值**

还有一种情况，如果返回的是一个`thenable`接口的对象，则这个对象可以提供给`then()`处理程序进行解包操作

```js
async function fn() {
	return null;
}
// fn 返回的是 成功promise，成功的值为 null

// 返回一个原始值 
async function foo() { 
 return 'foo'; 
} 
foo().then(console.log); 
// foo

// 返回一个没有实现 thenable 接口的对象
async function bar() { 
 return ['bar']; 
} 
bar().then(console.log); 
// ['bar']

// 返回一个实现了 thenable 接口的非期约对象
async function baz() { 
 const thenable = { 
 then(callback) { callback('baz'); } 
 }; 
 return thenable; 
} 
baz().then(console.log); 
// baz
```



2: 如果抛出错误`throw new Error('出错了')`

```js
async function fn() {
      throw new Error('出错了');
}
const result = fn();
console.log(result);//返回失败的promise对象 rejected
```

则async函数返回一个失败的promise对象



3：如果返回了一个promise对象

则asyce函数返回的promise状态和`return new Promise(...)`的这个对象状态一致

```js
//成功的promise
async function fn() {
      return new Promise((resolve, reject) => {
        resolve('成功了');
      })
}
const result = fn();
console.log(result);//返回成功的promise对象值为成功了
```

```js
//失败的promise
async function fn() {
      return new Promise((resolve, reject) => {
        reject('失败了');
      })
}
const result = fn();
console.log(result);//返回失败的promise对象值为失败了
```



## await

注意点：**await会阻塞后续代码执行**

总结：

1. await必须写在async函数中（“单相思”,async不是必须要await）
2. **await右边的表达式一般为一个promise对象**
3. await返回的是promise成功的**值**（注意！是值）
4. await的promise失败了，就会抛出异常用`try...catch`捕获处理

```js
// 成功的promise
const p = new Promise((resolve, reject) => {
      resolve('成功了');
    })

    async function fn() {
      let result = await p;
      console.log(result); //返回的是值：成功了
    }
	//调用fn
    fn();
```

**异常的捕获**

```js
// 失败的promise   
   const p = new Promise((resolve, reject) => {
      // resolve('成功了');
      reject('失败了');
    })

    async function fn() {
      // 失败的用try...catch
      // 内部捕获
      try {
        let result = await p;
        console.log(result);
      } catch (e) {
        // e是await抛出的失败的值
        console.log(e);// 返回失败了
      }
    }

    fn();

// 外部捕获
fn().catch(err => {
  console.log(err);
})
```



细致：await 关键字期待（但实际上并不要求）一个实现 thenable 接口的对象，但常规的值也可以。

如 果是实现 `thenable `接口的对象，则这个对象可以由 await 来“解包”。如果不是，则这个值就被当作 已经解决的期约。

示例：

```js
// 等待一个原始值 
async function foo() { 
 console.log(await 'foo'); 
} 
foo(); 
// foo

// 等待一个没有实现 thenable 接口的对象
async function bar() { 
 console.log(await ['bar']); 
} 
bar(); 
// ['bar']

// 等待一个实现了 thenable 接口的非期约对象
async function baz() { 
 const thenable = { 
 then(callback) { callback('baz'); } 
 }; 
 console.log(await thenable); 
} 
baz(); 
// baz 

// 等待一个期约
async function qux() { 
 console.log(await Promise.resolve('qux')); 
} 
qux(); 
// qux
```





## async与await的注意事项

一：如果await后方跟着一个基本数据类型，则会将这个基本类型包装成一个Promise

```js
async function func() {
	let data1 = await 123;
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('data1: ', data1);
	return data1;
}

let ret = func();
console.log('ret:', ret);
```

二：**await会阻塞后续的代码**

```js
async function func() {
	let data1 = await 123; // 之后的代码会被阻塞
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('data1: ', data1);
	return data1;
}

let ret = func();
// 这行console.log会优先执行
console.log('ret:', ret);
```



例如：

```js
// 求下列console.log()的执行顺序
async function func() {
	console.log('3')
	let data1 = await 123; // 之后的代码会被阻塞
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('4');// 4这里就被await阻塞了，因为await是异步的
	return data1;
}
console.log('1')
func();
console.log('2');

// 执行顺序1，3，，2，4
```



## 来自JavaScript高级程序设计的讲解 await

在async/await 中真正起作用的是**await**。async 关键字，无论从哪方面来看，都不过是一个标识符。 毕竟，异步函数如果不包含 await 关键字，其执行基本上跟普通函数没有什么区别：

示例：

```js
async function foo() { 
 console.log(2); 
} 
console.log(1); 
foo(); 
console.log(3); 
// 1
// 2
// 3
```



要完全理解 await 关键字，必须知道它并非只是等待一个值可用那么简单。JavaScript 运行时在碰 到 await 关键字时，**会记录在哪里暂停执行**。等到 await 右边的值可用了，JavaScript 运行时会向消息 队列中推送一个任务，这个任务会恢复异步函数的执行。

示例：

```js
async function foo() { 
 console.log(2); 
 await null; 
 console.log(4); 
} 
console.log(1); 
foo(); 
console.log(3); 
// 1 
// 2 
// 3 
// 4 
```

运行的工作流程：

(1) 打印 1； 

(2) 调用异步函数 foo()； 

(3)（在 foo()中）打印 2； 

(4)（在 foo()中）await 关键字暂停执行，为立即可用的值 null 向消息队列中添加一个任务； 

(5) foo()退出； 

(6) 打印 3； 

(7) 同步线程的代码执行完毕； 

(8) JavaScript 运行时从消息队列中取出任务，恢复异步函数执行； 

(9)（在 foo()中）恢复执行，await 取得 null 值（这里并没有使用）； 

(10)（在 foo()中）打印 4； 

(11) foo()返回。
