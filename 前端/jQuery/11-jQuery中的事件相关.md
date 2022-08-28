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





# jQuery的自定义事件

自定义事件需要通过on绑定方法的方式能够进行绑定

注意：**自定义事件只能通过自动触发，来进行事件触发（trigger和triggerHandler）**

语法：

```js
$('button').on('myClick', function() {
  alert('我是自定义事件')
})

// 触发自定义事件，两个方法都可以
$('button').trigger('myClick');
$('button').triggerHandler('myClick');
```





# jQuery中的事件命名空间

注意：它也必须只能通过on方式来绑定的事件才能有命名空间

作用：用一个标识，来标注该方法，在协同开发时，可以通过标记事件的方式，来表明函数的来源（就是谁写的）

语法：

```js
// 在事件名儿后面通过.跟上标识
$('.son').on('click.zs', function() {
	alert('zs click')  
})

$('.son').on('click.ls', function() {
	alert('ls click')  
})


// 如果想要分别触发不同标识的click事件，可以通过trigger
$('.son').trigger('click.zs');
```



## 命名空间的一些特点

**第一个：**利用trigger触发子元素带命名空间的事件，那么父元素带相同命名空间的事件也会被触发，而父元素不带命名空间的不会被触发

示例：

```js
$('.father').on('click.zs', function () {
  alert('父元素有命名空间');  // 4.会触发
})

$('.father').on('click', function () {
  alert('父元素没有命名空间的事件');  // 3.不会触发
})


$('.son').on('click.zs', function () {
  alert('子元素有命名空间事件'); // 2.会触发
})

// 1.自动触发带命名空间的子元素事件
$('.son').trigger('click.zs');
```



第二个：如果利用trigger触发常规事件，就是没有命名空间的事件，则所有对应的**同类型事件**都会被触发

注意：触发顺序是有命名空间的 ----> 没有命名空间的

示例：

```js

$('.father').on('click.zs', function () {
  alert('父元素有命名空间'); // 2.父元素的被触发
})

$('.father').on('click', function () {
  alert('父元素没有命名空间的事件'); // 3.父元素不带命名空间事件被触发
})

// 1.子元素被触发带命名空间事件被触发
$('.son').on('click.zs', function () {
  alert('子元素有命名空间事件');
})

// 1.触发不带命名空间click事件
$('.son').trigger('click');
```





# jQuery事件委托

什么是事件委托？

请别人帮忙做事，然后将做完的结果反馈给我们

事件委派jQuery有两种方法

核心：**利用了事件冒泡机制**

有几个注意点：

1. 委托，最主要的功能是，委托存在的元素，来帮忙触发，目前还不存在的元素身上的事件（**其实，任何一个有父子关系的标签都可以**）。
2. 尽量选择一些没有绑定过事件的元素进行委托



## delegate方法

该方法基本上已经启用，可以用on绑定事件来代替，但是这里还是讲一下吧。

语法：

```js
// 给ul绑定事件，然后委派给ul下的li元素，每个li都有一个click事件
$('ul').delegate('li', 'click', function () {
  console.log($(this).html());
})
```



## on方法

on方法不仅能够绑定事件，还可以委派事件，传递三个参数即可。

语法：

```js
// 传入三个参数，第一个：委派的事件名，第二个委派对象，第三个委派事件
$('ul').on('click', 'li', function () {
  console.log($(this).html());
})
```





**综合示例：**

```html
  <script>
    $(function () {
      $('button').on('click', function () {
        $('ul').append('<li>新增的li</li>');
      })


      // 这种方式，新增的li 就无法点击
      $('ul>li').on('click', function () {
        // 调用$(this)身上的方法
        console.log($(this).html());
      })

      // 事件委派方式1：利用delegate方法(弃用)
      $('ul').delegate('li', 'click', function () {
        console.log($(this).html());
      })


      // 事件委派方式2：利用on事件绑定
      $('ul').on('click', 'li', function () {
        console.log($(this).html());
      })
      
      // 注意：不一定非要委托到ul上，也可以委托到body上，body也是li的父父元素(●'◡'●)
       $('body').on('click', 'li', function () {
        console.log($(this).html());
      })
    })
  </script>
</head>

<body>
  <ul>
    <li>我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
  </ul>

  <button>增加li</button>
</body>
```





# jQuery鼠标移入移出事件

测试基础代码：

```html

  <style>
    .father {
      width: 100px;
      height: 100px;
      background-color: red;
    }

    .son {
      width: 50px;
      height: 50px;
      background-color: #bfc;
    }
  </style>
</head>

<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```





## mouseover方法

作用：当鼠标移入一个绑定的dom元素时，会被触发

语法：

```js
$('.father').on('mouseover', function () {
  console.log('鼠标移入father');
})
```



## mouseout方法

作用：当鼠标移出一个dom元素时被触发

语法：

```js
$('.father').on('mouseout', function () {
  console.log('鼠标移出father');
})
```



## mouseover和mouseout的小问题

mouseover和mouseout虽然能够监听移入和移出方法，但是他们俩有一点点儿的问题。当有嵌套关系时，移入/移出子元素，也会触发父元素的事件（即使子元素在父元素内）

所以一般我们都比较喜欢使用下面这三个方法



## mouseenter方法

作用：也是鼠标移入事件，但是没有奇怪的小问题

语法：

```js
$('.father').on('mouseenter', function () {
  console.log('鼠标移入father');
})
```



## mouseleave方法

作用：鼠标移出事件

语法：

```js
$('.father').on('mouseleave', function () {
  console.log('鼠标移入father');
})
```



注意：**这两个方法，就不会出现移入移出子元素触发父元素鼠标事件的情况**



## hover方法

作用：是mouseenter和mouseleave的结合版，传入两个回调，用来简写的。

语法：

```js
$('.father').hover(function () {
  console.log('移入father') // 回调函数1对应mouseenter
}, function () {
  console.log('移出father') // 回调函数2对应mouseleave
})
```

注意：这个方法只能通过eventName方式绑定哦，on绑定无效
