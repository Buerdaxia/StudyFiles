# jQuery

核心：Write Less，Do More！！！

1.x：兼容ie678，但是相对它版本文件较大

2.x：不兼容ie678，但是当对于1.x文件较小

3.x：不兼容ie678，只支持最新的浏览器，很多老的jQuery插件不支持这个版本，

## 使用jQuery

下载jQuery库	=>	引入jQuery	=>	编写jQuery代码



## jQuery的核心函数$()

$();就代表调用jQuery的核心函数

```js
// $();就代表调用jQuery的核心函数

// 1.接收一个函数,就是入口函数
$(function() {
    alert('hello qbr');
    // 2.接收一个字符串
    // 2.1接收一个字符串选择器,返回一个jQuery对象,对象中保存着找到的dom元素
    let $box1 = $('.box1');
    let $box2 = $('.box2');
    console.log($box1);
    console.log($box2);

    // 2.2接收一个字符串代码片段,返回一个jQuery对象,对象中保存着找到的dom元素
    let $p = $("<p>我是段落</p>");
    console.log($p);
    $box1.append($p);

    // 3.接收一个DOM元素
    // 会被包装成一个jQuery对象,对象中保存着找到的dom元素
    var span = document.getElementsByTagName('span')[0];
    var $span = $(span);
    console.log($span);
})
```

## jQuery对象是啥？

jQuery对象是一个伪数组。

啥是伪数组？就是有0-length-1的属性，并且有length属性。



## jQuery（原生js）的静态方法和实例方法的创建

静态方法，添加在类上，可以直接添加也能通过static关键字添加，通过类进行调用。

实例方法，添加在原型对象上，通过实例进行调用。

```js

// 1. 定义一个类
function AClass() {

}
// 2.给这个类添加一个静态方法
// 直接添加给类名
AClass.staticMethod = function() {
    alert('staticMethod');
}
// 通过类名调用静态方法
AClass.staticMethod();


// 3.给类添加一个实例方法
AClass.prototype.instanceMethod = function() {
    alert('instanceMethod');
}
// 创建一个实例
let a = new AClass();
// 通过实例调用实例方法
a.instanceMethod();
```

## jQuery的each方法和原生JS的forEach方法

```js
let arr = [1, 3, 4, 5, 7];
// 原生JS的forEach方法
/*
第一个参数:遍历到的元素
第二个参数:当前遍历到的索引
注意点:
原生forEach方法只能遍历数组,不能遍历伪数组
*/
arr.forEach(function(value, index){
    console.log(index, value);
})


// jQuery的each方法
/*
第一个是指定数组
回调函数中:
第一个参数:当前遍历到的索引
第二个参数:遍历到的元素
注意点:
jQuery的each是可以遍历伪数组
*/
$.each(arr,function(index, value) {
    console.log(index, value);
});
```

## jQuery的map方法和原生JS的map方法

```js
let arr = [1, 3, 4, 5, 7];
let obj = {0:1, 1:2, 2:4, 3:5, length:4};
// 原生JS的map方法
/*
第一个参数:遍历到的元素
第二个参数:当前遍历到的索引
第三个参数:当前被遍历的数组
注意点:
原生map方法只能遍历数组,不能遍历伪数组
*/
arr.map(function(value, index, array) {
console.log(index, value, array);
});
// jQuery的map方法
/*
第一个是指定数组
回调函数中:
第一个参数:遍历到的元素
第二个参数:当前遍历到的索引
注意点:
jQuery的map也是可以遍历伪数组
*/
$.map(arr,function(value, index) {
console.log(index, value);
});
$.map(obj,function(value, index) {
console.log(index, value);
});
```

## jQuery中的each与map方法区别

和原生JS中的forEach方法和map方法区别一致

```js
/*
    区别一:
    默认条件下,
    map返回空数组,
    each返回遍历的数组
*/

var res = $.map(obj,function(value, index) {
    console.log(index, value);
});
var res2 = $.each(obj,function(value, index) {
    console.log(index, value);
});
console.log(res);
console.log(res2);

/*
    区别二:
    each方法不支持在回调中对数组进行处理
    map方法可以在回调函数中通过return对遍历的数组进行处理并返回一个新数组
*/

var res = $.map(obj,function(value, index) {
    console.log(index, value);
    return value + index;
});
var res2 = $.each(obj,function(value, index) {
    console.log(index, value);
});
console.log(res);
console.log(res2);
```



## jQuery的其他静态方法

### trim方法

```js
/*
返回值 = $.trim(参数);
作用:去除字符串两端的空格
参数:要去除空格的字符串
返回值:去除空格后的字符串
*/
let str = "   qbr   ";
var res = $.trim(str);
console.log('---'+res+'---');
```

### isWindow(),isFunction,isArray()

```js
// 真数组
let arr = [1, 3, 4, 5, 7];
// 伪数组
let arrLike = {0:1, 1:2, 2:4, 3:5, length:4};
// 对象
let obj = {"name": '钱不二', age: 22};
// 函数
let fn = function() {};
// window对象
let w = window;
/*
返回值 = $.isWindow(对象);
作用:判断传入的对象是否是window对象
返回值: Boolean
*/
let res = $.isWindow(w);
console.log(res);

/*
返回值 = $.isArray(对象);
作用:判断传入的对象是否是真数组
返回值: Boolean
*/
let res2 = $.isArray(arr);
console.log(res2);

/*
返回值 = $.isArray(对象);
作用:判断传入的对象是否是函数
返回值: Boolean
注意点:
jQuery框架本质上是一个函数
*/
let res3 = $.isFunction(jQuery);
console.log(res3);//true
```

**jQuery框架本质上是一个函数**



### $.holdReady()方法

如果想要先执行其他框架或者代码，让jq最后执行，就可以使用$.holdReady()方法

```js
/*
$.holdReady(true);
作用:暂停执行
$.holdReady(false);
作用:恢复执行
*/
$.holdReady(true);//下面代码被暂停
$(function() {
alert("ready");
})


let btn = document.getElementsByTagName('button')[0];
btn.onclick = function() {
    alert("btn");
    $.holdReady(false);//执行恢复
}
```

## jQuery的内容选择器

```js
/*
:contains(text)
:has(selector)
:parent
:empty
*/

// :empty 作用：找到即没有文本内容也没有子元素的指定元素
let $div = $("div:empty");
console.log($div);

// :parent 作用：找到有内容的（不是空的就行）指定元素
let $div2 = $("div:parent");
console.log($div2);

// :contains(text) 作用：找到包含指定文本内容的指定元素
let $div3 = $("div:contains('我是div')");
console.log($div3);

// :has(selector) 作用：找到包含指定子元素的指定元素
let $div4 = $("div:has('span')");
console.log($div4);
```

## jQuery中获取属性节点attr与removeAttr

```js
/*
1.attr(name|pro|key,val|fn);
作用：获取或设置属性节点的值
获取：
传递一个参数：代表获取属性节点的值
注意点：
无论找到多少个元素，都只会返回第一个元素指定的属性节点的值

设置（修改）：
传递两个参数：代表设置属性节点的值
注意点：
找到多少元素就设置多少元素
有则修改，无则添加 


2.removeAttr(name)
删除属性节点
注意点：
会删除所有元素指定的属性节点
*/

// 获取span元素上的class值
console.log($("span").attr("class"));

// 修改所有span上的class值为box
console.log($("span").attr("class", "box"));

// 删除span元素上的class属性节点
$("span").removeAttr("class");

// 删除class 和 name 属性节点
$("span").removeAttr("class name");
```

## jQuery中prop()和removeProp()方法

```js
/*
操作属性
1.prop方法
特点和attr方法一致
2.removeProp方法
特点和removeAttr一致

注意：
此方法不仅能操作属性，还能操作属性节点

官方推荐：在操作属性节点时，具有true和false两种属性的节点，例如checked
selected或者disabled时使用prop(),其他时间使用attr()
*/

// 添加属性demo    
$("span").eq(0).prop("demo", "ahha");
//    console.log($("span").prop("demo"));

// 删除demo属性
$("span").removeProp("demo");
//    console.log($("span").prop("demo"));

//操作属性节点 操作属性节点的方式和attr一致
$("span").prop("class");//获取class属性节点

console.log($("input").prop("checked"));//true
console.log($("input").attr("checked"));//checked
```

官方推荐：**在操作属性节点时，具有true和false两种属性的节点，例如checked、selected或者disabled时使用prop(),其他时间使用attr()**

## jQuery中操作类的方式，添加，修改，切换类

```js
/*
    1.addClass(class|fn)
    作用：添加一个类
    如果要添加多个，多个类名之间用空格隔开

    1.removeClass(class|fn)
    作用：删除一个类
    如果要删除多个，多个类名之间用空格隔开

    1.toggleClass(class|fn)
    作用：切换类，
    有就删除，没有就添加
*/
let btns = document.getElementsByTagName("button");

btns[0].onclick = function () {
    let div = document.getElementsByTagName("div")[0];
    $("div").addClass("box1");
};

btns[1].onclick = function () {
    let div = document.getElementsByTagName("div")[0];
    $("div").removeClass("box1");
};

btns[2].onclick = function() {
    let div = document.getElementsByTagName("div")[0];
    $("div").toggleClass("box1 box2");
```

## jQuery中的文本操作html，text，val

```js
/*
1.html([val|fn])
和原生JS中的innerHTML是一样的

1.text([val|fn])
和原生JS中的innerText是一样的

1.val([val|fn|arr])
和原生JS中的value属性差不多
*/

// 设置html
$("div").html("<p>嘿嘿嘿</p>");
//获取html
$("div").html();

// 设置text //只能获取设置内容，一样拿不到标签
$("div").text("<span>haha ha</span>");
// 获取text
$("div").text();

// 设置value
$("input").val("请输入...");
// 获取value
$("input").val();
```

## jQuery中操作CSS属性方法

```js
// 1.逐个设置
$("div").css("width", "100px");
$("div").css("height", "100px");
$("div").css("background", "red");

// 2.链式操作
// 如果链式操作大于3步，建议分开
$("div").css("width", "100px").css("height", "100px").css("background", "red");

// 3.批量设置
$("div").css({
    width: "100px",
    height: "100px",
    background: "red"
})

// 4.获取CSS样式
console.log($("div").css("width"));
```

## jQuery获得尺寸的方法width，innerWidth，outerWidth

width = 内容宽度

innerWidth = 内容宽度+ padding

outerWidth = 内容宽度 + padding + border + margin

高度 一致

```js
//获取指定元素的内容宽度，高
$(".father").width()
$(".father").height()

```

## jQuery中获取位置的方法offset，position

```js
// offset([coordinates])
// 作用：获取元素距离窗口的偏移量
console.log($(".son").offset().left);

// 设置元素距离窗口的偏移量
$(".son").offset({
left: 10
});

// position()
// 作用：获取元素距离定位元素的偏移量(定位父元素)
console.log($(".son").position().left);

// 注意点：position方法只能获取不能设置
$(".son").position({
    left: 10
});//无效
```

注意点：**position方法不能设置**

## jQuery中的滚动条方法scrollTop

```js
// 获取滚动条的偏移量
console.log($(".scroll").scrollTop());
// 获取浏览器的滚动条偏移量
console.log($("html").scrollTop());
// 注意点：ie浏览器是body 为了兼容
// 兼容写法：
console.log($("html").scrollTop()+$("body").scrollTop());


// 设置滚动条的偏移量
$(".scroll").scrollTop(300);//后面写number

// 设置网页滚动条偏移量
// 注意点：为了兼容ie 要以如下方式写
$("html,body").scrollTop(300);
```

**水平轴滚动条，scrollLeft设置方法一致**

## jQuery的事件绑定

```js
/*
jQuery有两种绑定事件方式
1.eventName(fn);
部分事件jQuery没有添加,所以不能实现

2.on(eventName, fn);
所有js事件都可添加(自我建议使用)

注意点:
两种方式都可以可以添加多个不同或者相同事件,不会覆盖
推荐使用第二种
*/

// 方式一:
$("button").click(function() {
alert("hello");
});

// 方式二:
$("button").on("click", function() {
alert("hello 2");
})
```



## jQuery的事件解绑off方法

```js
function test1() {
alert("hello  1");
}
function test2() {
alert("hello 2");
}
$("button").click(test1);
$("button").on("click", test2);

$("button").mouseleave(function() {
alert("hello mouseleave");
})
$("button").mouseenter(function() {
alert("hello mouseenter");
})

// off方法如果不传递参数,会移除指定元素身上所有事件
$("button").off();
// off方法如果只传递一个参数,会移除元素身上指定事件
$("button").off("click");
// off方法如果传递两个参数,会移除元素身上指定事件的指回调函数
$("button").off("click", test1);
```

## jQuery中的事件冒泡和默认行为取消

**事件冒泡和原生JS一样**

**默认行为的取消也和原生JS一致**

```js
/*
1.什么是事件冒泡?
相同事件,会从儿子到祖先一直触发
2.如何阻止事件冒泡?
方式一:在儿子事件中返回false
方式二:在儿子事件中写event.stopPropagation();

3.什么是默认行为?
例如a标签的跳转,button的提交等
4.如何阻止默认行为  
方式一:return false;
方式二:event.preventDefault();
*/
$(".son").on("click",function(event) {
alert("son");
// return false;
event.stopPropagation();
})

$(".father").on("click",function() {
alert("father");
})


$("a").on("click", function(event) {
alert("弹出注册框");
// return false;
event.preventDefault();
})
```

## jQuery中自动触发事件trigger和triggerHandler

```js
$(".son").on("click",function(event) {
alert("son");
})

$(".father").on("click",function() {
alert("father");
})
/*
trigger: 如果使用trigger自动触发事件,会触发事件冒泡
triggerHandler: 如果使用triggerHandler自动触发事件,不会触发事件冒泡
*/
$(".son").trigger("click");
$(".son").triggerHandler("click");

$("input[type='submit']").on("click", function() {
alert("submit");
});

/*
trigger: 如果使用trigger自动触发事件,会触发默认行为
triggerHandler: 如果使用triggerHandler自动触发事件,不会触发默认行为
*/
$("input[type='submit']").trigger("click");
$("input[type='submit']").triggerHandler("click");
```

注意点：

a元素比较特殊，利用trigger去自动触发a元素绑定的事件，并不会触发默认行为，需要结合冒泡才能触发a元素默认事件

```js
//想要触发自动触发a的默认事件写法
//必须嵌套一个span 利用冒泡来解决
<a><span></span><a>
$("span").click(function() {
	alert("a");
});
$("span").trigger("click");
```



## jQuery中的自定义事件

```js
/*
自定义事件的条件:
1.自定义事件需要利用on绑定事件
2.自定义事件需要trigger触发或者triggerHandler触发
*/
//创建了一个名字为myClick的事件
$(".son").on("myClick",function(event) {
alert("son");
})
//利用trigger触发
$(".son").trigger("myClick");
```



## jQuery中事件的命名空间

jQuery的on帮定事件，可以在事件名后加上标识，来标记这个事件的作用，或者提供者等有效信息。

```js
/*
命名空间的条件:
1.利用on绑定事件
2.通过trigger触发事件
*/
$(".son").on("click.zs",function(event) {
alert("click zs");
});

$(".son").on("click.ls",function(event) {
alert("click ls");
});

$(".son").trigger("click.zs");
```

-------------------------------

```js
//命名空间的特点
$(".father").on("click.ls",function(event) {
alert("father lsClick");
});
$(".father").on("click",function(event) {
alert("father click");
});
$(".son").on("click.ls",function(event) {
alert("son lsClick");
});
/*
1.利用trigger触发子元素带命名空间的事件，那么父元素带相同命名空间的事件也会被触发
而父元素没有命名空间的事件，则不会被触发

2.利用trigger触发子元素不带命名的事件，父元素相同类型的事件都会被触发
*/
$(".son").trigger("click.ls");
$(".son").trigger("click");
```



