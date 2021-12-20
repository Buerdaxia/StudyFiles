# this

前景：任何函数本质上都是通过某个对象来调用的~

所有函数内部都有一个变量this，它的值是调用函数的当前对象。



# this到底是谁

1. this 是以普通function()函数调用时，this是window(这是在非严格模式下)。就是没有直接指定的就是window
2. this 以对象来调用时，this就是这个对象
3. this 在事件的响应函数中，响应函数是给谁绑定的this就是谁

## 怎么确定this的值

1. test()： window
2. p.test()：p
3. new test()：新创建的对象
4. p.call(obj)：obj

