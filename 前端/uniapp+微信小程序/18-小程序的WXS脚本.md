# 小程序的`WXS`脚本

WXS（WeiXin Script）是小程序独有的一套脚本语言，结合WXML，可以构建出页面的结构。



## WXS的应用场景

在`wxml`中无法调用在页面的`.js`中定义的函数，但是，`wxml`中可以调用`wxs`中定义的函数。因此，小程序中`wxs`的典型应用场景就是**过滤器**



## WXS脚本的特点

1）wxs借鉴了JavaScript，但是wxs还是和JavaScript是两种不同的语言，不能无脑搬js代码

2）**不能作为组件的事件回调**

wxs典型的应用场景就是“过滤器”，经常要配合Mustache语法使用，例如：

```html
<!-- 使用m1模块的wxs -->
<view>{{m1.toUpper(username)}}</view>
```

注意：wxs中定义的函数不能作为组件的事件回调，以下代码就是**错误的**：

```html
<button bindtap='m1.toUpper'>
  按钮
</button>
```



3）隔离性

隔离性指`wxs`的运行环境和其他JavaScript代码是隔离的，体现在如下两个方面：

1. `wxs`不能调用`js`中定义的函数
2. `wxs`不能调用小程序提供的`API`



4）**性能好**

在IOS设备上，小程序内的WXS会被JavaScript代码快2~20倍。

在android设备上，差不多。



## 基本语法

### 一：内嵌wxs脚本

wxs代码可以**编写在wxml文件中的<wxs>标签内**，就像JavaScript编写在<script>里一样



注意：wxml文件的每个<wxs>标签，必须提供module属性，用来指定当前wxs的模块名称，方便wxml在模块中访问的成员。

示例：

```html
<!--username是在data中定义好的-->

<!-- 使用m1模块的wxs -->
<view>{{m1.toUpper(username)}}</view>
<!-- 定义一个wxs  -->
<wxs module="m1">
  module.exports.toUpper = function(str) {
    return str.toUpperCase();
  }
</wxs>
```





### 二：外联的wxs脚本

**定义**：

wxs代码也可以直接写在`.wxs`为后缀名的文件内，就像JavaScript代码可以编写在`.js`为后缀名的文件按一样

示例：

```js
//  /utils/tools.wxs
function toLower(str) {
  return str.toLowerCase()
}

module.exports = {
  // 注意：这里wxs不支持es6简写，必须写全
  toLower: toLower
}
```



**使用**：

在wxml中引入外联wxs脚本时，必须为<wxs>标签添加module和src属性：

* `module`用来指定模块名称
* `src`用来指定要引入的脚本路径，**必须是相对路径**

示例：

```html
<view>{{m2.toLower(country)}}</view>

<!-- 引入外部定义的wxs，并命名为m2 -->
<wxs src="../../utils/tools.wxs" module="m2"></wxs>
```

