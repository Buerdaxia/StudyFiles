# BFC

bfc全名：block formatting context

块级格式化上下文，是页面上用于渲染CSS的一个区域，相当于一个小型的布局，块级元素和浮动元素会根据这块区域进行布局。



## 功能，作用！

1. 清除浮动
2. 包裹浮动
3. 避免边距塌陷



## 设置bfc的方式

一、设置为float属性的元素。

二、绝对定位的元素：position: absolute。

三、设置display: inline-block。

四、设置overflow的值为除visible和clip以外的，如hidden

五、flex和grid布局的子元素（非flex和grid容器本身）



## 最常用的，也是最好用的

第一种：设置overflow的值为除visible和clip以外的，如hidden

第一种的问题：overflow会影响到滚动条



**第二种：设置display: flow-root**

display: flow-root属性的详解：

这个属性的全程是display: block flow-root;顾名思义就是一对外显示为block，对内创建文档布局流(自身作为该布局流的根)，而这个布局流构成的上下文就是bfc。

display: inline-block也能开启浮动的原因是，这个属性全称叫display: inline flow-root;