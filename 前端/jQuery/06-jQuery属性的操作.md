# jQuery属性的操作

先了解几个概念：

什么是属性？

对象身上保存的一个变量就是属性对吧。

如何操作属性？

用`.`对吧，对象.属性名来进行操作或者对象['属性名称']



**什么是节点属性？**

在编写HTML时，在一个标签中添加的一些属性，就叫做节点属性

示例：

```html
<input name="qec" id="1" />
<!--name,id都是节点属性，保存在input这个Dom元素的attributes属性下-->
```



**如何操作节点属性？**

原生JS通过两个Dom的方法`DOM元素.setAttributes('属性名称', '属性值');`、`DOM元素.getAttributes('属性名称')`

注意：这两个方法是直接将属性添加进Dom节点上，也添加进attributes里

示例：

```html
<body>
  <div name="jack">
    123
  </div>
</body>
<script>

  let div = document.getElementsByTagName('div')[0];
  console.log(div.getAttribute('name')) // jack
  div.setAttribute('id', '1');
  console.log(div.getAttribute('id')) // 1
</script>
```



jQuery中也封装了一套用来操作属性的方法，下面来一一介绍：



## jQuery操作节点属性（操作attributes）

以下这两个方法，都是在操作Dom节点上的attributes属性，对这里面的值进行删除和添加

### attr方法

作用：获取或者设置节点属性，**传递一个参数代表获取属性节点的值，传递两个参数代表设置节点属性**

语法：

```js
jQuery对象.attr(name|key, val|fn);
```

**获取属性节点的值：**

注意点：**无论找到了多少个元素，该方法只会返回第一个元素指定的节点属性的值**

示例：

```html
  <script>
    $(function () {
      // 找到了两个div
      const $div = $('div');
      // 但是只输出了jack，也就时第一个div的name
      console.log($div.attr('name'));
    })
  </script>
</head>

<body>
  <div name="jack">
    123
  </div>

  <div name="tom">
    456
  </div>
</body>
```



**设置节点属性的值：**

注意点：用该方法进行设置时，是找到多少个节点，设置多少个

示例：

```html
  <script>
    $(function () {
      // 找到了两个div
      const $div = $('div');
      // 会同时给两个div都添加上class="box1"
      $div.attr('class', 'box1');
    })
  </script>
</head>

<body>
  <div name="jack">
    123
  </div>

  <div name="tom">
    456
  </div>
</body>
```

### removeAttr方法

删除指定的节点属性。

语法：

```js
jQuery对象.removeAttr('属性名')
// 如果删除多个属性名，属性名用空格隔开
jQuery对象.removeAttr('属性名1 属性名2 属性名3')
```

注意点：**该方法会删除所有找到的节点对应的节点属性**

示例：

```html
  <script>
    $(function () {
      // 找到2个div
      const $div = $('div');
			// 给2个div同时添加class="box1"
      $div.attr('class', 'box1');
			// 两个div的class属性，name属性都被移除
      $div.removeAttr('class name');
    })
  </script>
</head>

<body>
  <div name="jack">
    123
  </div>

  <div name="tom">
    456
  </div>
</body>
```



## jQuery操作Dom节点属性

**这两个方法，都是在操作Dom节点上的属性，要和上面的两个方法进行区分**

### prop方法

作用：prop方法可以用来直接操作Dom节点身上的属性(设置/获取)

语法：

```js
// eq(0)表示选中第一个等同于$('span')[0]
$('span').eq(0).prop('属性名', '属性值');

// 传递一个参数是获取
$('span').eq(1).prop('属性名');
```



### removeProp方法

作用：删除prop新增的属性

注意：它也是找到多少删除多少

语法：

```js
$('span').removeProp('属性名');
```



### prop方法来操作attributes

prop也能够操作attributes中的属性，它也可以修改或添加`class`和`id`，但是我们不建议用它来操作，为啥原因看下面的使用建议

示例：

```js
// 修改id为box1 如果该Dom节点没有，会添加一个id='box1'
$div.eq(0).prop('id', 'box1');

// class和id一样,如果原来标签上有，会修改为box2，如果没有回添加一个class="box2"
$div.eq(0).prop('class', 'box2');
```



## prop和attr方法的使用建议

先看一小段儿代码：

```html
<script>
  $(function () {
    console.log($('input').prop('checked')); // true
    console.log($('input').attr('checked')); // checked字符串
  })
</script>
</head>

<body>
  <input type="checkbox" checked>
</body>

<!--如果是下面这种，没有checked的-->
<script>
  $(function () {
    console.log($('input').prop('checked')); // false
    console.log($('input').attr('checked')); // undefined
  })
</script>
</head>

<body>
  <input type="checkbox">
</body>
```

看完上面这一小段儿代码，大概你也知道该使用什么了：官方原话

**重点：**在操作节点属性时，具有`true`和`false`的两个属性的属性节点，如`checked`，`selected`或者`disabled`时使用：`prop`

其他使用：`attr`

