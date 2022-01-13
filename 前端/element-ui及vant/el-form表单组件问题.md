# el-form表单组件问题

## 问题一

使用表单验证时，出现警告，并且不显示验证效果。

浏览器控制台：

```
[Element Warn][Form]model is required for validate to work!
```

解决方式：

1. 检查`el-form`表单绑定数据时，是否将`:model`写为`v-model`，element-ui表单绑定数据使用`:model`
2. 检查`el-form`表单的ref属性，看是否出现重名

