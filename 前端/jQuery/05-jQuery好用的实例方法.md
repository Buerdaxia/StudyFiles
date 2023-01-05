# jQuery好用的实例方法



## eq方法

语法：

```js
$('div').eq(1);
// 选中jQuery伪数组对象索引为1的那个
jQuery伪数组：
{
  0: 真实的dom对象,
  ...,
  length: ..
}
```

作用：**通过eq取出的dom对象，会被jQuery包裹一遍，变成jQuery对象**

注意：这个方法非常有用

对比：

```js
$('div')[0]; //这样取出的是没有被$()包裹的真实dom
$('div').eq(0); // 这样取出的也是第0个，但是是被$()包裹了，可以使用jQuery对象所有方法
```



## index方法

语法：

```js
jQuerydom对象.index();
```

作用：找到该jQuery对象，在jQuery伪数组中的索引值

示例：

```js
// 这里假设找到3个div
$('div').click(function() {
  // 这里的this，就是点击的那个div
  $(this).index(); //就会输出点击的那个div在整个$('div')的到伪数组的索引
})
```



## siblings方法

语法：

```js
jquery对象.siblings();
```

作用：获取当前对象其他兄弟dom元素（就是获取jQuery伪数组中，自己除外的其他元素）过滤用的

示例：

```js
$('.nav>li').mouseenter(function () {
  // 获取当前dom外的其他元素，返回一个伪数组
  console.log($(this).siblings());
})
```





## children方法

语法：

```js
jQuery对象.children(参数)
```

作用：选择对象下的子元素，返回的是一个jQuery对象

示例：

```html
<ul class="nav">
    <li>一级菜单 <span>></span>
      <ul class="sub">
        <li>二级菜单</li>
        <li>二级菜单</li>
        <li>二级菜单</li>
        <li>二级菜单</li>
        <li>二级菜单</li>
      </ul>
    </li>
<ul>
  <script>
    $(function () {
      // 1.监听1级菜单点击事件
      $('.nav>li').on('click', function () {
        // 2.拿到对应1级下的二级菜单sub
        let $sub = $(this).children('.sub')

        // 3.让二级菜单展开
        $sub.slideDown(1000);

        // 4.选中其他所有的二级菜单
        // 选中所有其他一级下的二级sub
        let $otherSub = $(this).siblings().children('.sub')

        // 5.让其他的二级折叠上去
        $otherSub.slideUp(1000)
      })
    })
  </script>
```

