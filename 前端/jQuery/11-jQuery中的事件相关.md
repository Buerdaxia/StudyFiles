# jQuery中的事件绑定

jQuery中有两种绑定事件方式

1. $('xx').eventName(fn);

2. $('xx').on(eventName, fn);

推荐：**推荐使用on来绑定，on是一种万能的绑定方式任何情况下都不会出错，唯一缺点编码太长**

## eventName(fn)方法

和原生的onclick那种绑定方式很像，但又不是完全一致

示例：

```js
$('button').click(function () {
  alert('点击1');
})
$('button').click(function () {
  alert('点击2');
})

// 注意这里绑定同一个事件，原生中就会被覆盖掉，但是jq的是不会覆盖的
```





## on(eventName, fn)方法

on的一些优势，对于一样动态元素，你只能通过on来帮，第一种就不行

示例：

```js
$('button').on('click', function() {
  alert('我被点击了')
})
$('button').on('click', function() {
  alert('我被点击了2')
})

// 它也不会被覆盖
```





# jQuery的事件解绑

解绑的一个关键方法off，它可以不传参数，传递1个参数，2个参数



## off方法

一：**如果off方法不传递参数，则会解绑jQuery对象身上绑定的所有方法。**

语法：

```js
$('button').off();
// 移除button按钮身上所有绑定的事件
```







二：**如果传递一个参数，则是移除元素身上绑定的所有该事件**

语法：

```js
$('button').off('click');
// 移除button按钮身上绑定的所有click事件
```





三：**传递2个参数，则是移除指定类型的指定事件**

语法：

```js
$('button').off('click', test1);
// 移除button身上绑定的名叫test1的click事件


// button的绑定事件
function test1() {
  alert('我是test1');
}
function test2() {
  alert('我是test2');
}

// 绑定了两个点击事件
$('button').click(test1);
$('button').click(test2);
```





# jQuery事件冒泡和默认行为

## 事件冒泡现象：

同类型事件会像泡泡一样，从里向外一步一步冒泡到父亲，并且会触发所有同类型的事件。

注意：**这个现象是默认的**

## 阻止事件冒泡

**注意：阻止事件冒泡的方法，都是写在最里面，儿子里面的**

第一种：直接返回在绑定的事件中返回false，就会阻止默认冒泡事件

示例：

```js
$('son').on('click', function(e) {
  ....
  
  return false; // 最后return false就会阻止冒泡
})
```



第二种：调用`event.stopPropagation()`方法

示例：

```js
// 注意事件要传递一个参数event
$('son').on('click', function(event) {
  ....
 	event.stopPropagation() // 调用阻止事件冒泡
})
```





## 默认行为：

一些标签，本身自己就带有一些行为，例如标签`<a>`的默认行为就是跳转，表单`<form>`的默认行为就是提交表单



##  阻止默认事件

阻止默认事件也有两种阻止方式：

第一种：也是直接返回`false`

示例：

```js
$('a').on('click', function() {
  ....
  return false; //就会阻止a标签默认跳转的行为
})
```



第二种：`event.preventDefault()`方法来阻止，和原生一样

示例：

```js
$('a').on('click', function(event) {
  ....
  event.preventDefault(); // 调用方法，阻止跳转行为
})
```





# jQuery自动触发事件

jQuery中封装了两个可以自动触发事件的函数。



## trigger方法

作用：不用手动触发，在页面加载时就能够将事件进行触发

注意：**该方法既能触发冒泡，也能触发默认行为**

语法：

```js
$('.son').trigger('click');
// 自动触发类名是son的元素身上绑定的click方法
// 页面一加载就会触发
```

**小问题：**

```js
// 当使用trigger触发a标签时，默认是不会触发a标签跳转的默认事件的
$('a').on('click', function () {
  alert('跳转到百度');
})

$('a').trigger('click');
// 这样写，click事件会被触发，但是不会跳转

/* <body>
  <a href="http://www.baidu.com">
    <span>跳转</span>
  </a>
</body>
*/

// 如果想要触发，这样写，需要利用事件冒泡现象，会进行跳转的默认事件

// 给a标签下的span绑定事件
$('span').on('click', function () {
  alert('跳转到百度');
})
// 自动触发
$('span').trigger('click');
```







## triggerHandler方法

作用：triggerHandler方法和上面的trigger方法功能一样，就是触发后的效果有些不同。

注意：**该方法自动触发事件，既不会导致事件冒泡，也不会触发默认事件**

示例：

```js
$(function () {
  // 自动触发事件有两个trigger, triggerHandler
  $('a').on('click', function () {
    alert('跳转到百度');
  })

  // $('a').trigger('click');

  $('a').triggerHandler('click');// 该方法不会触发冒泡，和默认事件
})
```

