# jQuery的选择器

jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。

jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 [CSS 选择器](https://www.runoob.com/cssref/css-selectors.html)，除此之外，它还有一些自定义的选择器。

jQuery 中所有选择器都以美元符号开头：$()。来自：菜鸟教程



**几个注意点：**

第一个：**:** 即为 jQuery 的过滤选择器，语法类似于 css 中的伪类选择器；其过滤选择器大概可以分为基本过滤（p:first 之类）、内容过滤（:empty）、子元素过滤(:first-child)和属性过滤 **[href]** 选择器。

**过滤选择器，一般都是配合别的选择器一起使用：例如**

```js
$("div:empty") // 得到所有空的div标签
$("span:first-child") //得到第一个sapn子元素
```





第二个：**$(":button")** 为 jQuery 中表单选择器（貌似与过滤选择器同级），旨在选择所有的按钮，所以会找到 **<input>**、**<button>** 元素；而 **$("button")** 则为基本选择器，旨在选择为 **<button>** 的标签。



示例：

```js
$("#id", ".class")  复合选择器

$("div p span")       层级选择器 //div下的p元素中的span元素

$("div>p")            父子选择器 //div下的所有p元素

$("div+p")            相邻元素选择器 //div后面的p元素(仅一个p)

$("div~p")            兄弟选择器  //div后面的所有p元素(同级别)

$(".p:last")          类选择器 加 过滤选择器  第一个和最后一个（first 或者 last）

$("#mytable td:odd")      层级选择 加 过滤选择器 奇偶（odd 或者 even）

$("div p:eq(2)")    索引选择器 div下的第三个p元素（索引是从0开始）

$("a[href='www.baidu.com']")  属性选择器 // 这个是选择属性为href="www.baidu.com"的a标签

$("p:contains(test)")        // 内容过滤选择器，包含text内容的p元素

$("div:empty")        //内容过滤选择器，所有空标签的div（不包含子标签和内容的标签）parent 相反

$("div:parent")    //找到div标签下，有文本或子元素的div标签


$(":hidden")       //所有隐藏元素 visible 

$("input:enabled") //选取所有启用的表单元素

$(":disabled")     //所有不可用的元素

$("input:checked") //获取所有选中的复选框单选按钮等

$("select option:selected") //获取选中的选项元素
```



## 内容选择器

内容选择器原生JS我好像没见过，这里就写一下：

第一个：**xxx:empty**

作用：空内容选择器，**所有内容是空的标签都会被选中**（但是有空格的不算空），自闭合标签也算空（例如:input）

示例：

```js
$("div:empty")
// 标识，选中所有div的空div标签 例如这样的：<div></div>
```





第二个:**xxx:parent**

作用：和empty刚好相反，**它选中所有内容不是空的标签**

示例：

```js
let $div = $('div:parent')
// 选中所有有内容或子表标签的标签
```



第三个：**xxx:contains('内容')**

作用：选中内容包含传入字符串的标签

示例：

```js
let $div = $('div:contains("元素")')
// 选中div标签下，内容有元素两个子的标签
```





第四个：**xxx:has('标签')**

作用：选中一个子元素包含传入标签的元素（直接子元素，间接子元素都行，只要有就是）

示例：

```js
let $div = $('div:has("span")');
// 选中有span标签为后代元素的div

// <div><span></span></div> 这样的会被选中
// <div><head><span></span></head></div> 这样的也会被选中
```

