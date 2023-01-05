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



## 问题二：表单验证失效

我这里记录一种情况，就是表单数据层级嵌套时，出现的表单失效问题。

核心：**注意，如果想要进行表单验证，那么验证的数据只能在第一层**

**改正：这种嵌套数组，需要修改prop的值，为数组的值+index+键名🤭**

示例：

```js
let dataForm = {
  name: '', // 这两个在第一层的是可以进行验证的
  age: '',
  
  friend: {
    friend1: '', // 这两个就会失效
    friend2: ''
  }
  
  array: [
  	{
  		my: ''
		},
    {
      my: ''
    }
  ]
}
```



html部分：

```vue
<el-form :model="dataForm">
	<el-form-item prop="name" :rules="{required: true, message: '姓名不能为空', trigger: 'blur'}">
  	<el-input v-model="dataForm.name"></el-input>
  </el-form-item>
  
  <el-form-item prop="age" :rules="{required: true, message: '姓名不能为空', trigger: 'blur'}">
  	<el-input v-model="dataForm.age"></el-input>
  </el-form-item>
  <!--上面这两个是可以进行表单验证的-->
  
  
  <!-- 下面这种的大概率不不行-->
  <el-form-item v-for="(item, index) in dataForm.array" :prop="index" :rules="{required: true, message: '姓名不能为空', trigger: 'blur'}">
    <el-input v-model="item.my"></el-input>
  </el-form-item>
</el-form>
```





**解决办法**：

1. 要在单个的表单域上传递属性的验证规则
2. 可以在循环位置再套一层el-form标签，:model绑定的值就是循环的那个值

改正：**上面的解决办法有误，不应该这样做，应该是数组名+index+键名的方式来绑定prop，具体element-ui，form组件，动态表单哪里有讲**

## 问题三：动态表单的问题

**动态表单，必须要给每一个`el-form-item`来绑定一个`key`值。防止它表单验证时候出现重复验证的问题**

**2022-10-25：呃，改正一下，这个key绑不绑都可以(⊙﹏⊙)**

一般都是拿时间来当key值：

示例：

```js
let dataForm = {
  name: '',
  age: '',
  count: [
    {
      a: '',
      b: '',
      key: Date.now() // 这里要加一个key值
    },
    {
      a: '',
      b: '',
      key: Date.now()
    }
  ]
}

// 绑定
<el-form-item v-for="(item, index) in dataForm.count" :key="item.key"></el-form-item>
```



## 问题四：如果自定义label

我们可以使用el-form-item的插槽，来自定义label



示例：

```vue
<template>
	<lyl-form-item required>
    <!--这里使用插槽，插槽里面可以任意丢我们想要的元素-->
    <label slot="label">
      <span>姓名</span>
      <el-button type="primary" plain>选择人员</el-button>
  </label>
    <span style="color:#ff0000">共{{ 0 }}人</span>
    <lyl-input type="textarea" placeholder="请选择学时获得人员" :custom="true"></lyl-input>
  </lyl-form-item>
</template>
```

