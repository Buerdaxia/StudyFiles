# 异步函数策略（async/await小应用）



## 实现sleep

作用：非阻塞暂停

代码：

```js
async function sleep(delay) {
  return new Promise((resolve, reject)=> setTimeout(resolve, delay));
}


// 调用
async function foo() {
  const t0 = Date.now();
  await sleep(2000);
  console.log(Date.now() - t0);// 2004
}
foo();
```





## 利用平行执行

