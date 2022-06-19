# DOM0级和DOM2级

## DOM0级

语法：

```js
dom对象.事件名(onclick) = function() {
  xxx...
}
```

首先我们是把所有获去到的dom元素当作一个对象。

元素对象的基本结构：

```js
{
  id: '',
  className: 'xxx',
  style: 'xx',
  onclick: null,
  onmouseover: null
}
```

DOM0级的事件原理非常简单，就是直接给元素对象对应的事件行为的私有属性赋值

例如：

```js
box.onclick = function() {
  xxxx
}
// 就是在box元素对象的onclick上直接赋值 这个函数
```



## DOM2级

语法：

```js
dom对象.addEventListener(事件行为, 回调方法, 阶段)

box.addEventListener('click', callback, false);
```

DOM2级事件，有一个事件池的概念，事件池中存放着各种类型dom元素绑定的多个事件，当用`click`开始触发事件池中的事件时，会找到所有事件行为是`click`的事件，按照事件顺序依次执行



事件池模拟：

```js
元素		事件行为			方法			阶段
box 		click				fn1				false
box     click				fn2				false
box     mouseover		fn3				false
dom			click				fn5				false
...
```

当触发`box`的`click`事件后，浏览器会找到这个事件池，并依次执行所有元素为`box`，事件行为`click`的所有回调

