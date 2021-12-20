# table组件

## 表格属性:row-class-name=""

该属性可以控制表格显示样式，控制每行样式，不包括表头。可以传入一个对象或者一个函数。格式看官网。这里只写过程

一、确定样式

```css
//当组件中有scoped时，建议加上/deep/
// 表格显示样式
/deep/.el-table .warning-row {
  background: #ea8685;
}

/deep/.el-table .success-row {
  background: #7bed9f;
}
```

二、写处理函数

```js
  methods: {
    tableRowClassName({ row, rowindex }) {
        //row可以获得每行的数据，根据数据返回对应的样式类
      if (row.value === 0) {
        return 'warning-row '
      } else {
        return 'success-row'
      }
    }
  }
```

三、书写表格属性

```
//在表格属性位置添加
:row-class-name="tableRowClassName"
```



## 表格居中

表头居中：在el-table-column中添加属性

```
header-align="center"
```

表列及内容居中：也在el-table-column中添加属性

```
align="center"
```



