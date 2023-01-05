# 详情，编辑，新增三合一dialog

一般情况，我们都是会将这三个功能进行三合一，意思是这三个功能共用同一个界面（组件）。

三合一所带来的问题：

由于采用三合一方式进行书写，那么dialog中的表单就需要在每次重新打开时清空一次，并且还要重置验证规则。这里我们使用el-form自带的两个方法

示例：

```js
methods: {
  init(type, id) {
    this.type = type;
    this.id = id;
    
    this.$nextTick(() => {
      // 注意：这里对dataForm重新赋值一遍（就是初始值）
      // 初始化
      this.dataForm= {
        name: null,
        list: [], // 手动赋值
        ...
      }
      this.$refs.dataForm.resetFields(); // 将表单重置成初始值(也可以清空所有表单验证)      
    	
      if(this.id) {
				this.getInfo();
      }
    })
  }
}
```

