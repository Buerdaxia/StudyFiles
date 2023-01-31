# sass

sass有两种语法，一种是sass一种是scss，可以百度去查一下。



## sass的import

css有一个@import方式来引入其他css文件，sass也是一样的

语法：

```scss
@import "base/base";
```

### scss引入小贴士

scss会自动帮我们处理文件名：`_文件名.scss`引入时，既可以不用写`_`也可以用写后面的`.scss`

例如：目前的结构

```scss
sass
|
|---base
|			|
| 		|--_base.scss
|
|---main.scss

// 引入时 原文件名是_base.scss
@import "base/base";
```







## 变量声明

sass用$符号来声明变量（less用的是@）

示例：

```scss
$color-primary: #f9ed69;
$width-normal: 1000px;
```



使用：

```scss
$color-primary: #f9ed69;
$width-normal: 1000px;
button {
  // 这里rgba也能用
  background: rgba($color-primary, 0.8);
  width: $width-normal
}
```





## &字符

&字符的作用和less中的一样，都是代表当前的选择器

例如示例2：**巧妙的使用&符号，可以省略很多的代码**

示例：

```scss
div {
  color: red;
  &:hover { // 这里的&符号就是div选择器
    pointer: cursor;
  }
}
编译后：
div {
  color: red;
}

div:hover {
  pointer: cursor;
}
```



示例2： 

```scss
div {
  font-size: 14px;
  // &会替换当前类名
  &__logo {
    color: red;
  }
}
// 编译后
div {
  font-size: 14px;
}
div__logo {
  color: red;
}


```







## sass自带函数

### darken函数

作用：darken函数可以帮助我们将一个颜色变暗。

语法：

```scss
darken(参数一, 参数二);

// 参数一：颜色
// 参数二：变暗程度，可以写一个百分比
```



示例：

```scss
$color-primary: #f9ed69; // 黄色

.btn {
  background-color: $color-primary;
  &:hover {
    // 当触发hover时，将背景颜色变暗15%
    background-color: darken($color-primary, 15%);
  }
}
```



### lighten函数

作用：刚好和darken相反，让颜色变亮一定程度。

语法：

```scss
lighten(参数一, 参数二);
// 参数一：颜色
// 参数二：程度，百分比
```

示例：

```scss
$color-primary: #f9ed69; // 黄色

.btn {
  background-color: $color-primary;
  &:hover {
    // 当触发hover时，将背景颜色变亮15%
    background-color: lighten($color-primary, 15%);
  }
}
```



## sass混入mixin

就是将一些公用的scss代码抽离出来形成一个mixin，哪里需要，哪里就可以引入（思想和vue2中的mixin一致）



### 基础使用

基础语法：

```scss
// 创建mixin
@mixin clearfix {
  &::after {
    content: "";
    clear: both;
    display: table;
  }
}

// 引入
@include clearfix;
```



示例：

```scss
@mixin clearfix {
  &::after {
    content: '';
    clear: both;
    display: table;
  }
}
nav {
  margin: 30px;
  background-color: $color-primary;
  
  // 清除nav浮动
  @include clearfix;
}

// 编译过后
nav {
  margin: 30px;
  background-color: $color-primary;
}

nav::after {
  content: '';
  clear: both;
  display: table;
}
```





### 携带参数

mixin是可以携带参数的，和less的一样，都可以携带参数。

示例：

```scss
$color-text-dark: #000;

@mixin style-link-text($col) {
  text-decoration: none;
  text-transform: uppercase;
  color: $col;
}

// 使用
a {
  // 传入一个颜色
  @include style-link-text($color-text-dark)
}
```



### @content指令

@content作用使用来替代css代码，功能特别像上面的携带参数，但是它能带的更多，能把写入的代码块儿都能带过去。

基本使用：

```scss
// 再mixins.scss先定义了一个mixin
@mixin respond-phone {
  @media (max-width: 600px) {
    @content
  }
}

// 然后我们使用这个mixin
html {
	/* 浏览器默认16px， 我们要10px，并且计算为百分比 */
	font-size: 62.5%;
  
  @include respond-phone {
    font-size: 62.5%; // 这里的代码就会替换掉@content出现再mixins文件中
  }
}
```





## sass中的函数自定义function

sass中也可以自定义函数(虽然感觉用处不大)

基础语法：

```scss
// 创建
@function divide($a, $b) {
  @return $a / $b;
}


// 使用
nav {
  // *1px  就可以直接将30转换为30px
  margin: divide(60, 2) * 1px;
}
```

注意点：**函数的声明和返回都需要用`@`符号**





## sass的extend

extend的功能其实和mixin差不多，**只不过替换方式刚好相反。**

mixin是用代码替换掉@include的位置。

而extend使用选择器替换掉标识符。

示例：

```scss
// 这里我们利用%占位符将共同代码收集起来
%btn-placeholder {
  text-decoration: none;
  text-transform: uppercase;
  color: $color-text-light;
  padding: 10px;
  display: inline-block;
  width: 120px;
  text-align: center;
}


.btn--main {
  &:link {
    // 这里使用@extend符号， 其实这里就是将当前的选择器替换掉上面的%btn-placeholder占位符
    @extend %btn-placeholder; // 当前该位置的选择器是.btn--main:link
    background-color: $color-secondary;
  }
  
  &:hover {
    background-color: darken($color-secondary, 15%);
  }
}

.btn--hot {
  &:link {
    @extend %btn-placeholder;
    background-color: $color-tertiary;
  }
  
  &:hover {
    background-color: darken($color-tertiary, 15%);
  }
}
```



以上代码经过编译后的css代码：

```css
/*会用当前选择器，替换掉声明时候的选择器，这个就是extend的效果*/
.btn--main:link, .btn--hot:link{
   text-decoration: none;
  text-transform: uppercase;
  color: $color-text-light;
  padding: 10px;
  display: inline-block;
  width: 120px;
  text-align: center;
}


.btn--main {
  &:link {
    background-color: $color-secondary;
  }
  
  &:hover {
    background-color: darken($color-secondary, 15%);
  }
}

.btn--hot {
  &:link {
    background-color: $color-tertiary;
  }
  
  &:hover {
    background-color: darken($color-tertiary, 15%);
  }
}
```



## sass中使用calc函数

当在sass中使用calc并且使用了sass的变量时，必须要这样写：

```scss
// 必须在变量前用#{}包裹一下
$gutter-horizontal: 8rem;
width: calc((100% - #{$gutter-horizontal}) / 2);
```





## sass中的@if指令

if指令和JavaScript中的if功能一样，都是进行判断的。

基本使用：

```scss
@function divide($a, $b) {
  @if $a === $b {
    @return 1;
  }
  
  @if $a > $b {
    @return $a / $b;
  }
}
```



## sass中的媒体查询@media



在sass中可以直接这也写：

```scss
html {
  font-size: 62.5%;
  @media (max-width: 112.5em) {
    font-size: 75%;
  }
}

// 这也就会被编译为 标准css写法
html {
  font-size: 62.5%;
}

@media (max-width: 112.5em) {
  html {
    font-size: 75%;
  }
}
```



## sass编写相应管理器

使用@media、@if、@content

示例：

```scss
// 媒体查询
/*
  0 ~ 600px:  手机
  600px ~ 900px:  纵向平板
  900px ~ 1200px:  横向平板

  1200px ~ 1800px:  正常显示屏，笔记本

  1800px + :  大屏
*/

/*
$breakpoint的选择：
- phone
- tab-port
- tab-land
- big-desktop

  1em = 16px
*/
@mixin respond($breakpoint) {
  @if $breakpoint == phone {
    @media (max-width: 37.5em) {    // 600px
      @content
    }
  };

  @if $breakpoint == tab-port {
    @media (max-width: 56.25em) {     // 900px
      @content
    }
  };

  @if $breakpoint == tab-land {     // 1200px
    @media (max-width: 75em) {
      @content
    }
  };

  @if $breakpoint == big-desktop {
    @media (max-width: 112.5em) {     // 1800px
      @content
    }
  };
}
```

