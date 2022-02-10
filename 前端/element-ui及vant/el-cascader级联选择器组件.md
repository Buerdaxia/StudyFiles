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

