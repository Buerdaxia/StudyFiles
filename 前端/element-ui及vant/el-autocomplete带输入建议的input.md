# el-autocomplete带输入建议的input

```vue
    <el-autocomplete
      class="inline-input"
      v-model="state1"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
      @select="handleSelect"
    ></el-autocomplete>
```

state1: 输入值存储的地方

:fetch-suggestions="querySearch"：返回输入建议的方法，就是一获取焦点，就会自动调用该方法拿到建议数据。

@select="handleSelect"：当建议选中之后的操作

```vue
<script>
export default {
    name: 'xx',
    data() {
        return {
            // 分类条件的输入建议
      		conditionsAll: [],
        }
    }
},
mounted() {
    // 调用函数，传递输入条件
    this.conditionsAll = this.loadConditions();
},
methods: {
    // 该方法选中之后提交
	handleSelect(item) {
      console.log(item);
    },
    // 获取焦点时调用的函数
    querySearch(queryString, cb) {
      console.log(queryString);
      var conditionsAll = this.conditionsAll;
      console.log(conditionsAll);
      var results = queryString ? conditionsAll.filter(this.createFilter(queryString)) : conditionsAll;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    //  匹配条件函数，这个函数改动不大，基本就时修改indexOf一下
    createFilter(queryString) {
      return (item) => {
        return (item.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
      };
    },
    // 加载输入建议数据，这个可以根据接口来调用数据
    loadConditions() {
      return [
        {
          'value': 'router_name'
        }
      ]
    },
}
</script>
```



## 触发带输入建议的两种方式

###  1.输入框获取焦点时触发

默认就是这种

### 2.输入值后匹配触发

添加`:trigger-on-focus="false"`属性

```vue
    <el-autocomplete
      class="inline-input"
      v-model="state2"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
      :trigger-on-focus="false"
      @select="handleSelect"
    ></el-autocomplete>
```

## 使用注意注意点，输入建议的数据结构

输入建议中回调函数的结果是一个数组，里面放着对象，对象中有一个`value`属性，这是必须的,后面的无所谓

```js
[
  {"value": 'xxx', "xxx": 'xxx'}
]
```



但是如果拿到的数据不符合以上的结构，例如拿到的数据是这样的

```js
this.serverData =
[
  {id: '1', name: '123'},
  {id: '2', name: '456'}
]
```



可以使用`map`方法修改一下数据结构，注意（使用的是map方法奥）

```js
this.data = this.serverData.map((item) => {
  return {
    value: item.id,
    name: item.name
  }
})
```



## 增加回车触发事件

添加一个原生事件即可`@keyup.enter.native="handleSelect"`

```vue
    <el-autocomplete
      class="inline-input"
      v-model="state2"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
      :trigger-on-focus="false"
      @select="handleSelect"
      @keyup.enter.native="handleSelect"
    ></el-autocomplet
```



### 解决增加回车后输入建议框没有消失问题

如果遇到输入数据后，按下回车时，建议框没有消失，可以添加方法修改`el-autocomplete`类的属性，给他添加上`display: none`

添加input事件，和keyup事件

@input：当输入时触发事件

@keyup：当回车抬起来时触发



```
    <el-autocomplete
      class="inline-input"
      v-model="state2"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
      :trigger-on-focus="false"
      @select="handleSelect"
      @keyup.enter.native="handleSelect"
      @input="changeStyle('block', 'el-autocomplete')"
      @keyup="changeStyle('block', 'el-autocomplete')"
    ></el-autocomplet
```



然后添加`changeStyle(status, className)`方法

```js
// 展示建议框
changeStyle(status, className) {
  let d = document.getElementByClassName('className');
  d[0].style.display = status;
}
```



最后在`handleSubmit`方法的最后调用一下`changeStyle(status, className)`方法来修改样式隐藏建议框

```
handleSubmit() {
	.
	.
	.
  // 这是最后一条代码
  this.changeStyle('none', 'el-autocomplete');
}
```



## 表单验证问题

如果添加的是自带的验证效果，需要把`trigger: 'blur'`修改为`trigger: 'change'`

验证数组（rules）

```js
{ required: true, message: '请输入分类条件', trigger: 'change' }
```

自定义的没事



## 下拉建议框宽度问题

当想要修改下拉建议框大小时，盲目的修改样式无效

第一步，先在`el-autocomplete`中添加属性`:popper-append-to-body="true"`修改标签位置。（如果不修改的化，默认是将下拉框元素插入到body中的）

第二步：再来修改样式

```css
/deep/.el-autocomplete-suggestion {
    width: auto !important;
  }
// 这个是自动给建议框宽度，也可以添加固定宽度看自己
```

