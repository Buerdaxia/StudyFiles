# 纯函数(`redux`相关)

首先强调：<strong style='color: red'>redux中的reducer必须是一个纯函数</strong>

概念：一类特别的函数：只要是同样的输入(实参)，必定返回同样的结果。

实例：

```js
// 这里现在有个demo1为纯函数
demo1(1);// 返回1
// 过了一阵后。。。
demo1(1);// 任然返回1

// 不纯函数 demo2
demo2(1);// 返回的是1
// 过了一阵儿之后或者再次调用
demo2(1);// 返回的9
```



## 纯函数的约束

第一：不允许修改函数的参数

例如：

```js
function test(a) {
  a = 10;
  // 这里就修改了形参，test就不是一个纯函数
}
```



第二：不会产生任何副作用，例如：网络请求、输入输出设备（就是不靠谱的事儿我们(纯函数)不干）



第三：不能调用`Date.now()`或者`Math.random()`等这类不纯的方法



注意：**`redux`中的`reducer`函数必须是一个纯函数。**