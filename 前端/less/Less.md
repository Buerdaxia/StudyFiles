# Less

Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。这里呈现的是 Less 的官方文档（中文版），包含了 Less 语言以及利用 JavaScript 开发的用于将 Less 样式转换成 CSS 样式的 Less.js 工具。

## 来自b站大佬的评论：

运行时编译和预编译的区别：把编译看作一个单独的动作，预编译是将编译放在运行之前，由人工完成，再将编译好的css给到html，html运行时直接读到的就是css原生代码。运行时编译就是把编译和运行混合在一起，只有当浏览器打开 html 页面开始运行时才开始编译。举个剥瓜子的例子，直接嗑瓜子，就是把嗑和吃两个动作混合在一起，这就叫运行时编译，一边嗑一边吃，还有一种是先剥好瓜子壳，把瓜子仁全部放在一起，然后一口一个瓜子仁，这就叫预编译（预先剥好瓜子壳）

## vue中使用less

安装好相应版本的less-loader和less即可

less-loader要注意版本，高版本可能会不兼容

首先执行：

```
npm install --save less less-loader@5
```

其次要在样式层style加上lang="less"

```
<style lang="less" scoped>
</style>
```



## 一、less中的注释

less注释用：`//注释`写的注释，只能在less文件中看见，编译过后的css文件中不会显示。

less注释中用：`/*注释*/`写的注释，在less中和css文件中都能看到。

## 二、less中的@变量符

直接看，@声明普通属性值

```less
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

编译为-->

```css
#header {
  width: 10px;
  height: 20px;
}
```

---

注意：如果@声明选择器，或者属性名时

调用时要加上{},url时也需要用{}

```less
@selector: div;
@m: margin;
@{selector} {
 @{m}：10px
}
```

编译为--->

```css
div {
  margin: 10px;
}
```

总结：

选择器：@selector: xxx-->调用：@{selector}

属性名：@property: color -->调用：@{property}

url：@images: "../img" -->调用：@{images}

属性值：@link-color: #fff -->调用：@link-color

### less中的变量延时加载(唯一容易理解错的)

**less中的变量也是遵循着块级作用域的。**

**但是有一个问题就时，less会将整个块级作用域走完之后，才会加载变量。**

```less
@var: 0;
.class {
  @var: 1;
  .brass {
    @var: 2;
    width: @var; //这时编译后的width = 3
    @var: 3;
  }
  height: @var;
}
```

编译过后--->

```css
.class {
  height: 1;
}
.class .brass {
  width: 3;/*变量延时加载*/
}
```

## 三、less中的嵌套关系

less中比较特殊的就是能够使用嵌套的基本嵌套规则,这样就形成了一种父子级关系，如下列代码，div时span的父级

```less
div {
 span {
   xxx
 }
}
```

---

### &符的使用

嵌套好用是好用，但是就会有一定的问题,如上面代码 我想给span加一个hover效果怎么办？

```less
//这样写？
div {
 span {
   xxx
   :hover {
     xxx
   }
 }
}
```

上列代码编译过后--->

```css
div span {
  xxx
}
div span :hover {
  xxx;
}
/*发现有个问题，:hover并没有跟到span后面，而是做他的子元素了*/
```

---

那么解决以上问题，只需要加一个&符号即可

```less
div {
 span {
   xxx
   &:hover {//&符号表示当前选择器的父级元素
     xxx
   }
 }
}
```

## 四、less中的混合(很重要)

混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。

注意：**特别像函数奥**，但是，不是函数

### 混合的使用

```less
//将他俩共同的提取到这个定义的规则集中
.tbborder() {//加上()之后就不会在css中输出这段规则集
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
}

#border1 {
  width: 100px;
  height: 100px;
  background-color: #bfc;
  .tbborder();
}

#border2 {
  width: 200px;
  height: 200px;
  background-color: wheat;
  margin-top: 10px;
  .tbborder();
}
```

编译后的代码--->

```css
#border1 {
  width: 100px;
  height: 100px;
  background-color: #bfc;
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
}
#border2 {
  width: 200px;
  height: 200px;
  background-color: wheat;
  margin-top: 10px;
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
}
```

### 混合还能够传递参数

还是用上面的例子

注意：**参数可以带很多哦并且还能写默认参数和es6函数默认参数特别像**

```less
//传递了一个@color参数，并且带一个默认值为pink
.tbborder(@color:pink) {
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
  background-color: @color;
}

#border1 {
  width: 100px;
  height: 100px;
  .tbborder(#bfc);//传了一个#bfc作为颜色
}

#border2 {
  width: 200px;
  height: 200px;
  margin-top: 10px;
  .tbborder(wheat);//传了一个wheat小麦色
}

#border3 {
  width: 300px;
  height: 300px;
  margin-top: 10px;
  .tbborder();//没传值，那么就是默认值pink
}
```

编译过后--->

```css
#border1 {
  width: 100px;
  height: 100px;
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
  background-color: #bfc;
}
#border2 {
  width: 200px;
  height: 200px;
  margin-top: 10px;
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
  background-color: wheat;
}
#border3 {
  width: 300px;
  height: 300px;
  margin-top: 10px;
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
  background-color: pink;
}
```

### 混合中的命名参数

就是使用混合时候，可以指定要将值传递给那个参数

```less
.tbborder(@w:100px, @color:pink) {
  border-top: 2px solid black;
  border-bottom: 3px dotted red;
  background-color: @color;
  width: @w;
}

#border1 {
  height: 100px;
  .tbborder(100px, #bfc);
}

#border2 {
  height: 200px;
  margin-top: 10px;
  .tbborder(200px, wheat);
}

#border3 {
  height: 300px;
  margin-top: 10px;
  //这里指定传递参数值
  .tbborder(@color:black, @w:300px);
}
//编译过后的我就不写了吧,,ԾㅂԾ,,懂得都懂
```

### 混合中的匹配模式（很好用）

可以将面向对象变成一样，将混合写

```less
// 将重复的代码全部提取到一个同名混合中
// 只要这个混合第一个参数为@_，那么他就最先调用
.triangle(@_, @w, @color) {
  width: 0;
  height: 0;
  border: @w solid transparent;
}
//前面可以加一个字母，来标识这个函数的功能
//像这里的T指的是当前这个三角指向上面
.triangle(T, @w, @color) {
  border-bottom-color: @color;
}
//L是这个三角指向左边
.triangle(L,@w, @color) {
  border-right-color: @color;
}
.triangle(R,@w, @color) {
  border-left-color: @color;
}
.triangle(B,@w, @color) {
  border-top-color: @color;
}

#box {
  .triangle(B, 40px, pink);
}
```

### 混合中的arguments

arguments代表实参列表，也和js的arguments很像。

```css
.border(@w, @style, @color) {
  border: @arguments;
}

div {
  .border(1px, solid, red);
}

/*-->编译之后*/

div {
  border: 1px solid red;
}

```

## 五、运算

官网的说法

算术运算符 `+`、`-`、`*`、`/` 可以对任何数字、颜色或变量进行运算。如果可能的话，算术运算符在加、减或比较之前会进行单位换算。计算的结果以最左侧操作数的单位类型为准。如果单位换算无效或失去意义，则忽略单位。无效的单位换算例如：px 到 cm 或 rad 到 % 的转换。

```less
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%
```

乘法和除法不作转换。因为这两种运算在大多数情况下都没有意义，一个长度乘以一个长度就得到一个区域，而 CSS 是不支持指定区域的。Less 将按数字的原样进行操作，并将为计算结果指定明确的单位类型。

```
@base: 2cm * 3mm; // 结果是 6cm
```

## 六、less中的继承

继承：**性能比混合高，但是灵活度不如混合**

前提：首先只能写成没有参数的混合

其次结合extend关键字

```less
.juzhong {
  width: 100px;
  height: 100px;
  margin: 0 auto;
}
.juzhong:hover {
  background-color: red;
}
//利用:extend()关键字继承，后面的all，指继承.juzhong所有的效果
div :extend(.juzhong all) {
  background-color: pink;
}
```

编译之后--->

```css
/*发现div得到了.juzhong所有的内容*/
.juzhong,
div {
  width: 100px;
  height: 100px;
  margin: 0 auto;
}
.juzhong:hover,
div:hover {
  background-color: red;
}

div {
  background-color: pink;
}
```



## 七、less中的转义

转义（Escaping）允许你使用任意字符串作为属性或变量值。任何 `~"anything"` 或 `~'anything'` 形式的内容都将按原样输出。

```css
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

/*--->编译为*/
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

