# jQuery的入口函数(文档就绪事件)

jQuery的入口函数功能和原生的js功能基本一致。

入口函数：也叫**文档就绪事件**，只有当dom元素加载完毕时才会走该函数。

## 使用jQuery

下载jQuery库	=>	引入jQuery	=>	编写jQuery代码



第一个入口函数使用：

```js
// 1. 原生js的固定写法
window.onload = function(ev) {}

// 2. jQuery的写法
$(document).ready(function() {})
```

## jQuery入口函数的所有写法

```js
// 1.写法一
$(document).ready(function() {
    alert('hello jq');
});
// 2.写法二
jQuery(document).ready(function() {
    alert('hello jq');
});

// 3.写法三(推荐写法)
$(function() {
    alert('hello jq');
});

// 3.写法四
jQuery(function() {
    alert('hello jq');
});
```



## jQuery的入口函数与原生js入口函数(区别)

区别一：

1）**原生JS会等到dom元素加载完毕，并且图片也加载完毕才执行**



2）jQuery的入口函数只会**等到dom元素加载完毕就执行**，不会等到图片也加载完毕（所以拿不到图片的宽高）

```js
window.onload = function() {
    // 1.通过原生JS获取dom元素
    let img = document.getElementsByTagName('img')[0];
    console.log(img);
    
    // 2.元素JS可以拿到dom元素的宽高
    let width = getComputedStyle(img).width;
    console.log('onload:',width);
}

$(document).ready(function() {
    // 1.通过jQuery获取dom元素
    // let $img = $('img')[0];
    // console.log($img);

    // 2.jQuery不能拿到dom元素的宽高
    let $img = $('img');
    let $width = $img.width();
    console.log('ready:',$width);
})
```

区别二：

1）原生JS如果编写了多个入口函数,后面编写的会覆盖掉前面编写的

2）jQuery中编写的多个入口函数,会依次执行,不会覆盖

```js
// 1.原生JS如果编写了多个入口函数,后面编写的会覆盖掉前面编写的
// 2.jQuery中编写的多个入口函数,会依次执行,不会覆盖

window.onload = function() {
    alert('hello 1');
}
window.onload = function() {
    alert('hello 2');
}
// 2.jQuery中编写的多个入口函数,会依次执行,不会覆盖
$(document).ready(function() {
    alert('hello 1');
})
$(document).ready(function() {
    alert('hello 2');
})
```

