# H5 Web Workers

Web Workers是HTML5提供的一个JavaScript多线程解决方案，我们可以将一些大计算量的代码交给web Worker运行而**不冻结界面**，但是子线程完全受主线程控制，且不得操作DOM。

所以，这个新标准并没有改变JavaScript单线程的本质。



# 用法

主线程中语法：

`var worker = new Worker('分线程url');`

`worker.postMessage = 数据;`向分线程传如数据

``` 
worker.onmessage = function(event) {

​    // 主线程接收分线程返回的数据

​    console.log('主线程接收分线程返回的数据' + event.data);

​    alert(event.data);
}; 
```

分线程中语法：

``````javascript
var onmessage = function(event) {
    var number = event.data;
    console.log('分线程接收到主线程发送的数据'+ event.data);

    // 计算
    var result = 函数名(number);
    postMessage(result);
    console.log('分线程向主线程返回数据'+ result);

    // 分线程中的全局对象不在是window，所哟i分线程不能操作界面
}
function 函数名() {
    ....
}
``````

## 缺陷

1. 慢
2. 不能跨域加载js
3. worker内代码不能访问DOM（更新UI）因为分线程中的全局不在是window
4. 浏览器支持问题
