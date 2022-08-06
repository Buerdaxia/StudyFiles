## CSS特性

### 继承

css中有些属性是可继承的：一个元素如果没有设置某属性的值，就会跟随父元素的值

当遇到不能继承的属性，可以使用inherit值强制继承 `img {width:inherit;}` 

**继承注意点**：css继承的是计算值，而不是初始编码的最初值



### 层叠(选择器优先级)

css中允许多个相同名字的属性层叠到同一个元素上，最后结果就是:只有一个css属性会生效

1 相同选择器时，后面的会将前面的层叠掉

2 当选择器不同是，按照权重层叠

## css选择器权重

按经验来看(方便记忆)权重:

* !important：10000
* 内联样式：1000
* id：100
* 属性选择器，伪类，class：10
* 元素，伪元素：1



## css选择器

* 通用选择器
* 元素选择器
* 类选择器
* id选择器
* 组合选择器
* 属性选择器
* 伪类
* 伪元素

### 通用选择器

选择所有的元素：

`* {属性名: 属性值;}`

一般用于设置通用设置

### 类选择器(很重要)

`.class {...}` 可以很好的给样式进行分类和使用。

**强调！**：一个元素可以有多个类，每个类通过空格进行分隔，不要用标签名作为类名。

例：`<div class="box1 box2 box3 ...">div内容</div>`



**类的特殊写法**：

```css
/*
	标识如果一个标签中同时使用了这两个类（不论顺序，不论是否还有其他类，都应用里面的样式）
*/
.button.primary {
	background: #1da0ff;
}
```





类名规范：

1. 尽量见名知意
2. 当多个单词时候使用的连接方式：
   1. 用中划线 -  例：font-size
   2. 用下划线_ 例：font_size
   3. 驼峰法 例：fontSize(使用较少)

### id选择器

格式 `#id {属性名:属性值;...}`

**强调！**：id名称在同一个页面不要重复。

id名规范与类名规范一致。



### 属性选择器(较少)

格式`[属性名] { : ;}`

```css
/* 存在title属性的<a> 元素 */
a[title] {
  color: purple;
}

/* 存在href属性并且属性值匹配"https://example.org"的<a> 元素 */
a[href="https://example.org"] {
  color: green;
}

/* 存在href属性并且属性值包含"example"的<a> 元素 */
a[href*="example"] {
  font-size: 2em;
}

/* 存在href属性并且属性值结尾是".org"的<a> 元素 */
a[href$=".org"] {
  font-style: italic;
}

/* 存在class属性并且属性值包含以空格分隔的"logo"的<a>元素 */
a[class~="logo"] {
  padding: 2px;
}
```

### 后代选择器

格式 `标签 标签... {属性名:属性值;...}`   标签可以是id可以是类等等（中间是有一个空格的）

例：div span{属性名:属性值;}表示修改div下面的**所有**span的样式（直接或间接子类都可以）

注意：还有一种，两个类名挨在一起例如以下代码

```css
.panel.open {
  flex: 5;
  font-size: 40px;
}
/*指 如果panel后面有open这个类，则css属性生效*/
```



### 子选择器

格式 `标签>标签 {属性名:属性值;...} `   标签可以是id可以是类等等

例：div>span {}表示div元素里面的直接span子元素（**间接子元素不算**）

### 兄弟选择器

#### 相邻兄弟选择器

格式`标签1+标签2 {属性名:属性值;...}`

是同一个父元素时的元素才使用，并且这两个标签时相邻的。

**还要记住这个选择器，只选中第二标签，不管第一个标签的。**

```
<body>
<h1>This is a heading.</h1>
<p>This is paragraph1.</p>  -->这个p会变为红色
<p>This is paragraph2.</p>
<p>This is paragraph3.</p>
<p>This is paragraph4.</p>
<p>This is paragraph5.</p>
</body>

选中h1 后面紧贴着的p
h1 + p {
  color: red;
}
```

#### 通用兄弟选择器

格式`标签1~标签2 {属性名:属性值;...}`

同一个父元素，并且选择后续所有兄弟元素（可以不相邻）

```
<body>
<h1>This is a heading.</h1>
<p>This is paragraph1.</p>     //所有的p都会变红
<p>This is paragraph2.</p>
<p>This is paragraph3.</p>
<p>This is paragraph4.</p>
<p>This is paragraph5.</p>
</body>

选中h1 后面紧贴着的p
h1 ~ p {
  color: red;//选中h1的兄弟且p元素
}
```





### 交集选择器（重要）

格式 `标签标签 {: ;...}` 标签与标签之间没空格

例 div.one {} 要求通势符合2个条件的元素：div元素和class值有one



### 并集选择器（重要）

格式 `标签,标签 {: ;...}`  标签与标签之间用逗号，分隔

例：div, .one {} 表示所有div，class="one"的元素



### 伪类

#### :root

:root伪类选择器相当于html根元素，

等同于document.documentElement

```css
:root {
  xxx
}
// 等同于
html {
  xxx
}
```

