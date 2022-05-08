# 处理移动端1px被渲染2px问题

局部处理：

* 第一步：meta标签中的viewport属性，inital-scale设置为1；
* 第二步：rem按照设计稿标准走，外加利用transfrome的scale(0.5)缩小一倍即可。

全局处理：

* 第一步：meta标签中的viewport属性，inital-scale设置为0.5；
* 第二步：rem按照标准设计稿走就行辣