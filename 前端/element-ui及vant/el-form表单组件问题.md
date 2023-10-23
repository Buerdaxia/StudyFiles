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





**2022-10-25：呃，改正一下，这个key绑不绑都可以(⊙﹏⊙)，最好还是绑定一下，**

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



**校验失效的处理方法二：是在动态表单的v-for循环处，通过v-if进行判断，当所有要渲染字段从接口返回后，再进行渲染**

```vue
<template>
	<div>
    <el-form>
  		<template v-if="dataForm.fieldList.length > 0">
				<el-form-item v-for="(item, index) in dataForm.fieldList"></el-form-item>
			</template>
  	</el-form>
  </div>
</template>
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





## 问题四：动态表单无法输入值问题

有时候会遇到这样一个情况，表单要渲染的表单项内容，**是从接口进行返回的，需要等待接口返回完成后再进行渲染**，等待接口返回后，我们根据接口返回的内容，往`dataForm`数据中填入对应的表单项绑定的内容(v-model绑定的参数)，**会发现表单项怎么也不能进行输入**



问题原因：丢失响应性了

解决办法：在往`dataForm`中添加属性时使用`this.$set`

注意：**任何层级的新加属性都要通过`this.$set`来进行添加**



代码示例：

```vue
<script>
	export default {
    methods: {
      		getField() {
            this.loading = true; // 加载
            this.showForm = false; // 置动态表单渲染标识为false 注意点1：表单渲染标识
            let id = this.dataForm.germplasmId;
            this.$http.get(`${this.fieldUrl}?id=${id}`).then(({data: res}) => {
              this.loading = false;
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }

              this.fieldList = res.data.list;

              // 测试重写
              this.dataForm.fieldList = [];
              // 1.在fieldList中添加填写字段
              this.fieldList.forEach(item => {
                // 这里要判断一下是多选还是其他（多选是数组，其他是字符串）
                if (item.fieldType == 2) {
                  // 多选
                  this.$set(item, 'vModel', []);  // 注意点2：这里添加了一个字段，我们就用this.$set
                  // item.vModel = [];
                } else { 
                  this.$set(item, 'vModel', ['']);  // 注意点2：这里添加了一个字段，我们就用this.$set
                  // item.vModel = '';
                }

                this.dataForm.fieldList.push(item); // 注意点3：这里为什么可以不用this.$set是应为push被重写过可以使用
              });
              // this.$set(this.dataForm, 'fieldList', list);

              // 如果是编辑，需要将详情返回的值填入对应字段的vModel中
              if (this.dataForm.id) {
                for (let i = 0; i < this.dataForm.fieldList.length; i++) {
                  let isExist = false;
                  for (let j = 0; j < this.dataForm.breedingDataDetailsVoList.length; j++) {
                    if (
                      this.dataForm.fieldList[i].id == this.dataForm.breedingDataDetailsVoList[j].fieldId
                    ) {
                      if (
                        this.dataForm.breedingDataDetailsVoList[j].answerList &&
                        this.dataForm.breedingDataDetailsVoList[j].answerList.length == 0
                      ) {
                        // 如果返回的是空数组，说明从未填写过，需要重写判断并填入初始值
                        if (this.dataForm.fieldList[i].fieldType == 2) {
                          // 多选
                          this.$set(this.dataForm.fieldList[i], 'vModel', []);
                        } else {
                          // 其他
                          this.$set(this.dataForm.fieldList[i], 'vModel', ['']);
                        }
                      } else {
                        // id对应了
                        this.$set(
                          this.dataForm.fieldList[i],
                          'vModel',
                          this.dataForm.breedingDataDetailsVoList[j].answerList
                        );
                      }
                      // 置存在标记为true
                      isExist = true;
                      break;
                    }
                  }
                  if (!isExist) {
                    // 如果没有字段list和详情返回数据没有对应起来，就自定义一个vModel
                    if (this.dataForm.fieldList[i].fieldType == 2) {
                      // 多选
                      this.$set(this.dataForm.fieldList[i], 'vModel', []);
                    } else {
                      // 其他
                      this.$set(this.dataForm.fieldList[i], 'vModel', ['']);
                    }
                  }
                }
              }
              // console.log('表单字段', this.dataForm.fieldList);
              this.showForm = true; // 等待数据全部初始化完毕后，再渲染
            });
          },
    }
  }
</script>
```





