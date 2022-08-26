# jQuery类的操作

jQuery也封装了一套对类切换的方法，这三个方法对应着原生classList的那三个方法。





## addClass方法

作用：给一个dom元素添加一个类名

语法：

```js
$('div').addClass('类名')

// 添加多个类名，类名之间用空格隔开
$('div').addClass('类名1 类名2...')
```





## removeClass方法

作用：删除一个dom元素身上的类

语法：

```js
$('div').removeClass('类名')

// 删除多个也是一样，用空格隔开
$('div').removeClass('类名1 类名2')
```



## toggleClass方法

作用：切换一个类，指如果当前有该类，调用则移除，如果当前没有该类，调用则添加。

语法：

```js
$('div').toggleClass('类名')
```

