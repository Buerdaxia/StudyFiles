# 组件库vant-weapp

微信小程序好用的一个组件UI库

官网：https://vant-contrib.gitee.io/vant-weapp/#/home





## 使用css变量定制vant主题

核心：使用到了css3的变量

定义变量：--变量名

使用变量：var(--变量名)

注意：**css变量也是有作用域的，和选择器的嵌套层级有关。**

示例：

```css
/*html根节点，让整个页面都能访问定义的变量*/
html {
  --main-color: #c00;
}
.box1 {
  backgrount-color: var(--main-color);
}

.box2 {
  color: var(--main-color);
}
```





**定制全局主体样式**：

在app.wxss中，写入css变量，即可对全局生效：

示例：

```css
/*page是小程序中每个页面的根节点*/
page {
  /* 定制button type=danger的按钮颜色于边框颜色 */
  --button-danger-background-color: #C00000;
  --button-danger-border-color: #D60000;
}
```

具体变量值可以看`vant-weapp`官网：https://github.com/vant-ui/vant-weapp/blob/dev/packages/common/style/var.less

**vant-weapp变量是用less定义的，我们要用基础css变量来定义就需要将`@`改为`--`**

示例：

```css
@button-danger-background-color
/*改为*/
--button-danger-background-color 
```

