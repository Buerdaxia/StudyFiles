# el-select组件

## 问题一：数据从后端返回回显到页面无法进行删除操作

有时候有一种场景，就是编辑功能，选择组件数据从后端返回，并选择了一部分数据，但是发现怎么也无法对数据进行操作（例如删除）

解决方式：

>在el-select标签上绑`@change="$forceUpdate()"`

```vue
<el-select v-model="dataForm.enterpriseNature" @change="$forceUpdate()" multiple :options="enterpriseOptions" placeholder="企业性质" :info="typeList.indexOf(type) > -1"></el-select>
```



造成的原因：

回显的数据是从后台接口得来，由于数据层次太多，导致render函数没有自动更新；需要手动强制刷新

需要使用`@change="$forceUpdate()"`来强制刷新视图

