# async与await

## async

1. async函数的返回值是一个promise对象
2. promise对象的结果由async函数执行的返回值决定（就是由return语句决定）
3. 如果async函数状态没有改变，则返回一个`pending`状态`Promise`对象为（例如函数中有`await`，因为`await`会阻塞代码哟）

```
async function fn() {
      // 
      return '嘻嘻嘻'
}

```

三种情况：
1：如果返回的不是一个promise对象

则async函数返回的是一个成功的`promise`对象

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

二：await会阻塞后续的代码

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

