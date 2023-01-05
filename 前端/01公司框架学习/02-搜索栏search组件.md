# 搜索栏search组件

搜索栏search组件有一个固定的样式，是小屏4个，大屏6个。

其实在`<lyl-search-col>`中已经设置的值是这样的：`:lg="6" :md="6" :sm="6" :xs="6" :xl="4"`

所以是不用修改的，由于**时间选择器**的要求是**普通查询的长度的2倍**，所以我们需要修改一下时间选择器的：



示例：

```vue
<lyl-search slot="search" @search="listData">
  <!-- 普通的不需要修改 -->
  <lyl-search-col> <lyl-input v-model="dataForm.year" placeholder="登记年度"></lyl-input> </lyl-search-col>
  
  <!-- 时间相关的修改为普通的2倍 -->
  <lyl-search-col :lg="12" :md="12" :sm="12" :xs="12" :xl="8">
    <lyl-date-picker :default-time="['00:00:00', '23:59:59']" v-model="dataForm.registerTime" type="datetimerange" placeholder="登记" custom></lyl-date-picker>
  </lyl-search-col>
  <lyl-search-col> <lyl-select v-model="dataForm.stateCd" :options="stateOptions" placeholder="状态"></lyl-select> </lyl-search-col>
</lyl-search>
```

