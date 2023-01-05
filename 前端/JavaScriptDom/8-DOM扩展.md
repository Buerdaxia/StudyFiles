# DOM扩展

以下的是DOM扩展的方法，是在后面规范中一点一点加上来的。



## Selectors API

Selectors API（参见 W3C 网站上的 Selectors API Level 1）是 W3C 推荐标准，规定了浏览器原生支 持的 CSS 查询 API。



### querySelector()

querySelector()方法接收 CSS 选择符参数，返回匹配该模式的第一个后代元素，如果没有匹配 项则返回**null**。



示例：

```js
// 取得<body>元素
let body = document.querySelector("body"); 
// 取得 ID 为"myDiv"的元素
let myDiv = document.querySelector("#myDiv");
// 取得类名为"selected"的第一个元素
let selected = document.querySelector(".selected"); 
// 取得类名为"button"的图片
let img = document.body.querySelector("img.button"); 
```

在 Document 上使用 querySelector()方法时，会从文档元素开始搜索；在 Element 上使用 querySelector()方法时，则只会从当前元素的后代中查询。





### querySelectorAll()

querySelectorAll()方法跟 querySelector()一样，也接收一个用于查询的参数，但它会返回 所有匹配的节点，而不止一个。这个方法返回的是一个 NodeList 的**静态实例**。

再强调一次，querySelectorAll()返回的 NodeList 实例一个属性和方法都不缺，但它是一 个静态的“快照”，而非“实时”的查询。这样的底层实现避免了使用 NodeList 对象可能造成的性 能问题。

**所以说，有时候你用querySelectorAll是不如原生那些可以获得动态实例的方法好用的**

示例：

```js
// 取得 ID 为"myDiv"的<div>元素中的所有<em>元素
let ems = document.getElementById("myDiv").querySelectorAll("em"); 
// 取得所有类名中包含"selected"的元素
let selecteds = document.querySelectorAll(".selected"); 
// 取得所有是<p>元素子元素的<strong>元素
let strongs = document.querySelectorAll("p strong"); 
```





## 元素遍历(Element  Traversal 规范)

IE9 之前的版本不会把元素间的空格当成空白节点，而其他浏览器则会。这样就导致了 childNodes 和 firstChild 等属性上的差异。为了弥补这个差异，同时不影响 DOM 规范，W3C 通过新的 Element  Traversal 规范定义了一组新属性。

Element Traversal API 为 DOM 元素添加了 5 个属性：

* `childElementCount`：返回子元素数量（不包含文本节点和注释）；
* ` firstElementChild`：指向第一个 Element 类型的子元素（Element 版 firstChild）
* `lastElementChild`：指向最后一个 Element 类型的子元素（Element 版 lastChild）
* `previousElementSibling`：指向前一个 Element 类型的同胞元素（ Element 版 previousSibling）
* `nextElementSibling`：指向后一个 Element 类型的同胞元素（Element 版 nextSibling）。



所以说：IE还是很！！！



# `HTML5`规范的扩展



## `CSS`类扩展

### `getElementsByClassName()`

`getElementsByClassName()`是` HTML5 `新增的最受欢迎的一个方法，暴露在 document 对象和 所有 HTML 元素上。

`getElementsByClassName()`方法接收一个参数，即包含一个或多个类名的字符串，返回类名中 包含相应类的元素的 `NodeList`。

语法：

```js
// 取得所有类名中包含"username"和"current"元素
// 这两个类名的顺序无关紧要
let allCurrentUsernames = document.getElementsByClassName("username current"); 
// 取得 ID 为"myDiv"的元素子树中所有包含"selected"类的元素
let selected = document.getElementById("myDiv").getElementsByClassName("selected"); 

```





### classList属性

这个属性真的很好用。

要操作类名，可以通过 `className` 属性实现添加、删除和替换。但 `className `是一个字符串， 所以每次操作之后都需要重新设置这个值才能生效，即使只改动了部分字符串也一样。

例如：原先的处理

```html
<div class="bd user disabled">...</div> 
<script>
  // 要删除"user"类
  let targetClass = "user"; 
  // 把类名拆成数组
  let classNames = div.className.split(/\s+/); 
  // 找到要删除类名的索引
  let idx = classNames.indexOf(targetClass); 
  // 如果有则删除
  if (idx > -1) { 
   classNames.splice(i,1); 
  } 
  // 重新设置类名
  div.className = classNames.join(" "); 
</script>
```



`HTML5`通过给所有元素增加 `classList `属性为这些操作提供了更简单也更安全的实现方式。 `classList` 是一个新的集合类型 `DOMTokenList` 的实例。

**还有以下这4个方法：**

* `add(value)`，向类名列表中添加指定的字符串值 value。如果这个值已经存在，则什么也不做。
*  `contains(value)`，返回布尔值，表示给定的 value 是否存在。
* `remove(value)`，从类名列表中删除指定的字符串值 value。
* `toggle(value)`，如果类名列表中已经存在指定的 value，则删除；如果不存在，则添加。





#  `HTMLDocument` 扩展

## `readyState`属性

readyState 是 IE4 最早添加到 document 对象上的属性，后来其他浏览器也都依葫芦画瓢地支持 这个属性。最终，HTML5 将这个属性写进了标准。document.readyState 属性有两个可能的值：

*  loading，表示文档正在加载；
*  complete，表示文档加载完成。

以前没有这个属性的时候，一般都是我们自己写一个标记，来标识是否load完毕，现在可以使用这个属性了

示例：

```js
if (document.readyState == "complete"){ 
 // 执行操作
} 
```



## 自定义数据属性

HTML5 允许给元素指定非标准的属性，但要使用前缀 data-以便告诉浏览器，这些属性既不包含 与渲染有关的信息，也不包含元素的语义信息。除了前缀，自定义属性对命名是没有限制的，data-后 面跟什么都可以。下面是一个例子：

```html
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div> 
```

定义了自定义数据属性后，可以通过元素的 dataset 属性来访问。dataset 属性是一个 `DOMStringMap `的实例，包含一组键/值对映射。元素的每个 data-name 属性在 dataset 中都可以通 过 data-后面的字符串作为键来访问（例如，属性` data-myname、data-myName` 可以通过 `myname` 访 问，但要注意 data-my-name、data-My-Name 要通过` myName `来访问）。

示例：

```js
// 本例中使用的方法仅用于示范
let div = document.getElementById("myDiv"); 
// 取得自定义数据属性的值
let appId = div.dataset.appId; 
let myName = div.dataset.myname; 
// 设置自定义数据属性的值
div.dataset.appId = 23456; 
div.dataset.myname = "Michael"; 
// 有"myname"吗？
if (div.dataset.myname){ 
 console.log(`Hello, ${div.dataset.myname}`); 
} 
```





## 插入标记

DOM 虽然已经为操纵节点提供了很多 API，但向文档中一次性插入大量 HTML 时还是比较麻烦。 相比先创建一堆节点，再把它们以正确的顺序连接起来，直接插入一个 HTML 字符串要简单（快速） 得多。HTML5 已经通过以下 DOM 扩展将这种能力标准化了。

###  `innerHTML` 属性

在读取 innerHTML 属性时，会返回元素所有后代的 HTML 字符串，包括元素、注释和文本节点。 而在写入 innerHTML 时，则会根据提供的字符串值以新的 DOM 子树替代元素中原来包含的所有节点。



示例：

```js
div.innerHTML = "Hello world!"; // 这样只会简单插入文本节点

div.innerHTML = "Hello & welcome, <b>\"reader\"!</b>"; // 这样就会插入文本节点的时候还会放入一个b标签和一些转移字符
```



**注意**：**一般来讲，插入大量的新 HTML 使用 `innerHTML` 比使用多次 DOM 操作创建节点再插入来得更便捷。**





## `scrollIntoView`()方法

DOM 规范中没有涉及的一个问题是如何滚动页面中的某个区域。为填充这方面的缺失，不同浏览 器实现了不同的控制滚动的方式。在所有这些专有方法中，`HTML5` 选择了标准化 `scrollIntoView()`。



该方法接受两个参数：

* `alignToTop` 是一个布尔值
  * true：窗口滚动后元素的顶部与视口顶部对齐。
  *  false：窗口滚动后元素的底部与视口底部对齐。
*  scrollIntoViewOptions 是一个选项对象。
  * behavior：定义过渡动画，可取的值为"smooth"和"auto"，默认为"auto"。
  * block：定义垂直方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默 认为 "start"。
  *  inline：定义水平方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默 认为 "nearest"。

> 不传参数等同于 `alignToTop` 为 true。



示例：

```js
// 确保元素可见
document.forms[0].scrollIntoView(); 
// 同上
document.forms[0].scrollIntoView(true); 
document.forms[0].scrollIntoView({block: 'start'}); 
// 尝试将元素平滑地滚入视口
document.forms[0].scrollIntoView({behavior: 'smooth', block: 'start'}); 
```





# 专有扩展

尽管所有浏览器厂商都理解遵循标准的重要性，但它们也都有为弥补功能缺失而为 DOM 添加专有 扩展的历史。虽然这表面上看是一件坏事，但专有扩展也为开发者提供了很多重要功能，而这些功能后 来则有可能被标准化，比如进入 HTML5。



## children属性

IE9 之前的版本与其他浏览器在处理空白文本节点上的差异导致了 children 属性的出现。 children 属性是一个 HTMLCollection，只包含元素的 Element 类型的子节点。

**如果元素的子节点类型全部是元素类型**，那 children 和 childNodes 中包含的节点应该是一样的。





##  contains()方法

DOM 编程中经常需要确定一个元素是不是另一个元素的后代。IE 首先引入了 contains()方法， 让开发者可以在不遍历 DOM 的情况下获取这个信息。contains()方法应该在要搜索的祖先元素上调 用，参数是待确定的目标节点。

>IE来作用了

如果目标节点是被搜索节点的后代，contains()返回 true，否则返回 false。下面看一个例子：

示例：

```js
console.log(document.documentElement.contains(document.body)); // true 
```



## 插入标记

`HTML5` 将 IE 发明的 `innerHTML` 和 `outerHTML` 纳入了标准，但还有两个属性没有入选。这两个剩 下的属性是` innerText` 和 `outerText`。



### innerText 属性

`innerText` 属性对应元素中包含的所有文本内容，无论文本在子树中哪个层级。在用于读取值时，` innerText` 会按照深度优先的顺序将子树中所有文本节点的值拼接起来。在用于写入值时，`innerText` 会移除元素的所有后代并插入一个包含该值的文本节点。

简单来说：**要是单单只插入文本字符串，我们用innerText，如果文本字符串中包含着HTML元素，那我们用innerHTML**

小技巧：

因为设置 `innerText` 只能在容器元素中生成一个文本节点，所以为了保证一定是文本节点，就必 须进行 HTML 编码。`innerText` 属性可以用于去除 HTML 标签。通过将 `innerText`设置为等于 `innerText`，可以去除所有 HTML 标签而只剩文本，如下所示：

```js
div.innerText = div.innerText; 
```

