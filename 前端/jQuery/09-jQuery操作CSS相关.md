# jQuery操作CSS相关



## css方法

css方法也有两个功能，第一个设置，第二个获取

设置语法：

```js
// 1.方式一：逐个添加样式
$('div').css('width', '100px');
$('div').css('height', '100px');
$('div').css('background', 'red');

// 2.方式二：链式调用
// 注意：链式超过3个就不建议使用链式调用了
$('div').css('width', '100px').css('height', '100px').css('background', 'blue');


// 3.方式三：批量设置
$('div').css({
  width: '100px',
  height: '100px',
  background: 'green'
})
```

获取语法：

```js
// 传入属性名，返回属性值
$('div').css('width')

console.log($('div').css('width')) // 100px
```

