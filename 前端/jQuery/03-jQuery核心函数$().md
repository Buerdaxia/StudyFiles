# jQuery核心函数$()

`$()`这样就是在调用jQuery的核心函数。

示例：

```js
// 普通函数
function test() {
  xx
}
// 调用
test();


// jQuery一样， $就是一个函数，只不过jQuery帮我们封装好了而已
// 调用
$();
```



## 一：$()接收一个函数

当接收一个函数时，作用就是入口函数：

示例：

```js
// 1.接收一个函数,就是入口函数
$(function() {
    alert('hello qbr');
    // 2.接收一个字符串
    // 2.1接收一个字符串选择器,返回一个jQuery对象,对象中保存着找到的dom元素
    let $box1 = $('.box1');
    let $box2 = $('.box2');
    console.log($box1);
    console.log($box2);
})
```



## 二：$()接收一个字符串

接收一个字符串有两种情况，第一种接受一个选择器，第二种接收一个代码片段。



### 接收一个字符串选择器

字符串选择器和原生的差不多(querySelector)：

示例：

```js
// 接收两个选择器
let $box1 = $('.box1');
let $box2 = $('#box2');
```

**得到的是一个jQuery对象，对象中有存储找到的dom元素**





### 接收一段代码片段

如果接收一段代码片段，他会根据这段代码片段去**创建对应的dom元素**，返回的任然是**一个jQuery对象**

示例：

```js
// 2.2接收一个字符串代码片段,返回一个jQuery对象,对象中保存着找到的dom元素
let $box1 = $('.box1');
let $p = $("<p>我是段落</p>");
console.log($p);
// 在box1中添加一个元素
$box1.append($p);
```





## 三：$()接收一个Dom元素

如果传入一个原生Dom对象，则会返回一个被jQuery包装过的**jQuery对象。**

示例：

```js
// 3.接收一个DOM元素
// 会被包装成一个jQuery对象,对象中保存着找到的dom元素
var span = document.getElementsByTagName('span')[0];
var $span = $(span);
console.log($span);
```





## jQuery对象到底是什么鬼？

jQuery对象是一个伪数组。