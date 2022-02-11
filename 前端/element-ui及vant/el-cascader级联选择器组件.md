# el-cascader级联选择器组件

## 获取级联选择器的label项

首先获取`el-cascader`标签dom元素，先给标签绑定一个`ref`属性

获取label有两种方式：

element-ui 2.9.2版本之前

```
this.$refs['cascader'].currentLabels
```

element-ui 2.9.2版本之后

```
this.$refs['cascader'].getCheckedNodes()[0].pathLabels
```

标签完整代码

```vue
<el-cascader
  ref="cascader"
  ...
></el-cascader>
```



## 级联选择器的清空操作

也是先要绑定一个ref.

代码：

```vue
<el-cascader
  ref='cascader'
  ...
>
</el-cascader>


//清空方法
clear() {
  this.$refs.cascader.$refs.panel.clearCheckedNodes();
}
```

但是:还有一个小技巧

```
this.$refs.cascader.$refs.panel.getCheckedNodes()
```

该方法可以获得当前级联选择器选择的内容**数组**，我们可以在清空级联选择器时，先用该方法判断一下，级联选择器是否为空，如果已经为空了，在调用`clear()`方法就会报一个警告

处理方法

```js
clear() {
// 先判断一下级联选择器是否为空
if (this.$refs.cascader.$refs.panel.getCheckedNodes().length != 0) {
    	// 如果不为空则调用清空方法
        this.$refs.cascader.$refs.panel.clearCheckedNodes();
      }
}
```



