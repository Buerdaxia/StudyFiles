# Promise的几个关键性问题

## 1. 如何改变promise状态 

1.  resolve()：pending => resolved/fullfilled
2. reject()：pending => rejected
3. 抛出异常：如果当前为pending则变为rejected

## 2. promise指定的多个成功/失败的回调函数，都会调用吗？

当promise改变为对应状态时都会调用

指多个**then**在promise状态改变后，对应的回调函数都会执行

then()方法中promise状态不改变不会进行回调。

## 3. 改变promise状态和指定回调函数谁先谁后？

指的就是promise在执行时，是resolve()先执行,还是then()先执行?

两种情况都存在：

第一种：先改状态在执行then()

1. 在执行器中执行同步任务，直接调用resolve()/reject()
2. 延迟更长的直接给then()

第二种：先执行then()在改变状态

1. 在执行器中执行的是异步任务

   例如：有ajax呀，计时器呀等

**什么时候才能拿到数据？**

1. 如果先改状态后执行then()，先改变状态再执行then的回调函数，执行完毕就可以。
2. 如果先执行then()再改变状态，是在改变状态之后再去调用成功或失败的结果。

## 4. promise.then() 返回的新promise的结果状态由什么决定？

1）简单表达：由then()指定的回调函数执行结果决定。（**只要then方法不报错，返回的基本上都是成功的**）

2）详细表达：

1. 如果抛出异常，新promise变为rejected,，reason为抛出的异常
2. 如果返回的是非promise的**任意值**(包括undefined，null等等等...)，新promise为resolved/fullfilled，value为返回结果的值
3. 如果返回的是一个新promise，此promise的结果就会成为新promise的结果

## 5. promise 如何串连多个操作任务？

1. promise的then()返回一个新的promise，可以开成then()的链式调用。
2. 通过then的链式调用串连多个同步/异步任务。

## 6. promise的异常穿透

1. 当使用promise的then链式调用，可以再最后指定失败的回调(**只需要再最后指定一个失败回调就行，不用每个then都指定一个失败回调**)
2. 前面任何操作出了异常，都会传到最后失败的回调中处理

## 7. 如何中断promise 链？

如果当使用promise的then链式调用时，想要在中间中断，不再调用后面的回调函数！

方法：在回调函数中返回一个pending状态的promise对象
