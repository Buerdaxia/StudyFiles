# el-checkbox-group

一般`el-checkbox-group`是和`el-checkbox`多选一起使用的，`el-checkbox-group`可已将多选按钮分成一组一组的。



## 使用

```html
<el-checkbox-group v-model="tableHead">
              <el-checkbox v-for="(item,index) in 				showAttributes"
                :key="index"
                :label="item"></el-checkbox>
            </el-checkbox-group>
```

1 这里的`tableHead`绑定的是一个数组，是下面`el-checkbox`要选择的内容。

2 当勾选一个`checkbox`就会将label值push进这个`tableHead`数组。

3 当取消勾选时，就会将`tableHead`数组里对应的label值删除掉。

4 循环一般是在`el-checkbox`身上循环

5 绑定函数只能用`@change`不能使用`@click`



## 使用checkbox-group结合el-table

这两个组件结合可以实现一个动态显示表格列的效果

```vue
<el-checkbox-group v-model="tableHead">
              <el-checkbox v-for="(item,index) in 				showAttributes"
                :key="index"
                :label="item"></el-checkbox>
            </el-checkbox-group>

<el-table :data="tableData">
	<el-table-column v-for="(item, index) in tableHead"
          :key="index"
          :prop="item"
          :label="item"></el-table-column>
</el-table>
<script>
  export default {
    data() {
      return {
        tableHead: ['1', '2'],
        showAttributes: ['1', '2'],
        tableData: [{1: '我是1', 2: '我是2'}]
      }
    }
  }
</script>
```

当点击`el-checkbox`时，会动态的影响到`tableHead`的内容，`tableHead`又是渲染表格的循环数组，所以会动态的改变表格展示的列

