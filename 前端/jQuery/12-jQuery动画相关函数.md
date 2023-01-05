# jQuery动画相关函数

jQuery其实自己封装了几个和动画相关的函数，我觉得应该以后用的不多。

## 动画队列

jQuery中，有一个动画队列的概念，就是当调用一个动画方法时，动画进入队列，**只有当你到队头时才能执行**。

队列的核心：先进先出，队头执行



**动画队列有利有弊**

规律：**在jQuery中如果要使用动画函数，建议先调用一下stop方法**

## 显示与隐藏

### show方法

作用：调用后元素进行展示，但是有一些简单的展示动画

语法：

```js
// 前面是要展示的元素
/*
	参数一：动画持续时间，单位毫秒
	参数二：动画执行完毕后的回调
*/
$('div').show(1000, function () {
  alert('展示完毕')
});
```



### hide方法

作用：调用后元素进行隐藏，并且有隐藏动画

语法：

```js
// 前面是要隐藏的元素
/*
	参数一：动画持续时间，单位毫秒
	参数二：动画执行完毕后的回调
*/
$('div').hide(1000, function () {
  alert('隐藏完毕')
});
```



### toggle方法

作用：调用后自动在show和hide中切换

语法：

```js
$('div').toggle(1000, function () {
  alert('切换完毕')
});
```



## 展开与收起

### slideDown方法

作用：**将元素展开**（皮都展开了(●'◡'●)）

语法：

```js
$('div').slideDown(1000, function () {
  alert('展开完毕')
})
```



### slideUp方法

作用：**将元素进行折叠**

语法：

```js
$('div').slideUp(1000, function () {
  alert('折叠完毕')
})
```



### slideToggle方法

作用：也是在up和down中切换

语法：

```js
$('div').slideToggle(1000, function () {
  alert('切换完毕')
})
```



## 淡入淡出动画



### fadeIn方法

作用：让元素慢慢显示出来

语法：

```js
// 淡入动画
$('div').fadeIn(1000, function () {
  alert('淡入完毕')
})
```

### fadeOut方法

作用：让元素慢慢隐藏，是一个过渡的效果

语法：

```js
// 淡出动画
$('div').fadeOut(1000, function () {
  alert('淡出完毕')
})
```



###  fadeToggle方法

作用：淡入和淡出之间进行切换

语法：
```js
$('div').fadeToggle(1000, function () {
  alert('切换完毕')
})
```



### fadeTo方法

作用：让元素淡入到一个值，需要传入一个opacity的值

语法：

```js
// 注意，这个函数要传入3个参数，第二个参数是透明度
$('div').fadeTo(1000, 0.5, function () {
  alert('切换完毕')
})
```



## 自定义动画（重要）

`jQuery`虽然已经自带了上面那些方法，但是肯定是不够用的，这里就有自定义动画函数`animate`

自定义动画函数根据传入参数内容的不同，分为三种。

### animate方法

作用：自定义函数核心方法，可接受4个参数

参数一：一个对象，表示要操作的css属性与属性值

参数二：动画时间，单位是毫秒

参数三：字符串，动画执行方式，默认是swing（先慢后快再慢）可修改为linear匀速

参数四：回调函数

语法：

```js
$('div').animate({
  width: '200px' // 键值对儿方式
  // width: 200也可以
}, 1000, 'linear', function () {
  alert('执行完毕');
})
```

#### 操作属性

操作属性，意思指，直接对元素原来css属性进行单纯替换，从而达到从一个属性值过渡到另一个属性值

就像：原来元素`width: 100px`，现在直接修改变为`width: 500px`，这种叫做操作属性

语法

```js
<style>
    div {
      width: 100px;
      height: 100px;
      margin-bottom: 10px;
    }

    .one {
      background-color: red;
    }
</style>
$(function () {
  $('button').eq(0).on('click', function () {
    // 原来是width100px，执行后以动画方式变为width200px
    $('.one').animate({
      width: '200px'
    }, 1000, 'linear', function () {
      alert('执行完毕');
    })
  })
)}
```



#### 累加/减属性

累加属性，指对一个属性不断进行累加操作，是在原有值上进行不断累加。

核心：**在书写第一个参数时要带上`+=`或`-=`这种操作符**

示例：

```js
// 动画执行一次就会在原有width上减去100px
$('.one').animate({
  width: '-=100px'
}, 1000, function () {
  alert('累加执行完毕');
})

// 这个是加法
$('.one').animate({
  width: '+=100px'
}, 1000, function () {
  alert('累加执行完毕');
})
```



#### 关键字

关键字有`hide`，`toggle`这样的，`hide`代表英寸，`toggle`表示切换。

hide示例：

```js
// 执行动画，会将width变为0
$('.one').animate({
  width: 'hide' // 隐藏
}, 1000, function () {
  alert('累加执行完毕');
})
```



toggle示例：

```js
// 在有宽度和没有宽度之间切换
$('.one').animate({
  width: 'toggle'
}, 1000, function () {
  alert('累加执行完毕');
})
```





## 延迟函数

### delay方法

作用：用于延迟一段时间，一般在链式调用时候使用，主要就是在一个动画执行完毕后，想要延迟一段时间就可以调用该方法

语法：

```js
// 要求：先变宽，等待2s之后再变高
$('.one').animate({
  width: '200px'
}, 1000).delay(2000) // 在这里调用了延迟函数
  .animate({
  height: '200px'
}, 1000, 'linear', function () {
  alert('执行完毕');
})
```



## 停止动画

### stop方法

作用：停止当前**正在执行的动画**，可以传递两个参数，参数的不同功能也不同

语法：

```js
// 立即停止当前动画，继续执行后续动画
$('div').stop(); // 默认其实是false，false
$('div').stop(false);
$('div').stop(false, false);

// 立即停止当前动画和后续所有动画
$('.one').stop(true);
$('.one').stop(true, false);

// 3. 立即完成当前的，停止后续动画
$('.one').stop(true, true);

// 4，立即完成当前执行动画，继续执行后续的动画
$('.one').stop(false, true);
```



示例：

```js
$sub.stop();
// 在调用任何函数之前，都先调用一下stop函数
$sub.slideDown(1000);
```



