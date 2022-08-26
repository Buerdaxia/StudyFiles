# jQuery操作位置和尺寸



## 尺寸相关的API

### width方法

作用：width方法，获取/设置一个元素内容的宽度，这个宽度是不包含border，padding的，就单纯指的是**内容宽度**

语法：

```js
// 获取
$('.father').width()

// 设置,传入宽度
$('.father').width('500px')
```





### innerWidth方法

作用：该方法，是获取**内容宽度+内边距**

语法：

```js
// 获取
$('.father').innerWidth()

// 设置,传入宽度
$('.father').innerWidth('500px')
```



### outerWidth方法

作用：获取的宽度是 **内容宽度 + 内边距 + 边框**

语法：

```js
// 获取
$('.father').outerWidth()

// 设置,传入宽度
$('.father').outerWidth('500px')
```



### 对应的三个高度的方法，height

三个高度的方法计算方式和宽度一样。

分别是`height`、`innerHeight`、`outerHeight`





## 位置相关的API



### offset方法

作用：offset方法和原生那个一样，都是用来设置/获取元素相对于视口的偏移量。



语法：

```js
// 获取 4个值，left top right bottom
$('.son').offset().left //获取距离视口左侧的距离


// 设置
$('.son').offset({
  left: 10 // 传入一个对象，对象写入键值对，注意不带单位
})
```







### position方法

作用：获取元素距离定位元素的偏移量（这里基本上是获取决定定位的元素，然后能得到相对于父元素的距离）

注意：**该方法只能获取，不能设置**

语法：

```js
// 获取 当然它也有4个方向left top right bottom 
$('.son').position().left
```



## 滚动条相关



### scrollTop方法

作用：**获取/设置**滚动条相对于视口上面的偏移量，也就是垂直滚动条向下滚动的距离嘛

获取语法：

```js
// 获取元素的滚动距离
$('div').scrollTop()

// 获取整个页面的滚动距离
$('html').scrollTop()

// 兼容ie的写法，因为在ie浏览器中，认为滚动条是在body上的
$('html').scrollTop() + $('body').scrollTop()
```

设置语法：

```js
// 设置
$('div').scrollTop(300) // 设置滚动条滚动到300位置

// 设置整个页面的滚动条
$('html').scrollTop(300)

// 兼容ie
$('html,body').scrollTop(300)
```



### scrollLeft方法

作用：scrollLeft方法的使用语法和上面scrollTop一样，只不过获取和设置的是水平的滚动条位置，所以使用时**借鉴一下上面写法就行**，这里就不过多介绍了