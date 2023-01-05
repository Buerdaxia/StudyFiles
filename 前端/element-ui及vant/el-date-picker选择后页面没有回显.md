# el-date-picker选择后页面没有回显

出现这个问题时候，可以打印下数据，会发现数据是没啥问题的，就是显示有问题。



解决方式有两种：

```vue
<lyl-date-picker
 @change="edit(topic)"
 @input="changeDate(topic, $event)"
 v-model="topic.answer"
 :type="date"
 :format="yyyy-HH-mm-dd"
 ></lyl-date-picker>

<script>
	export default {
    methods: {
      changeDate(topic, e) {
      // 一：用$set解决
			// this.$set(topic, 'answer', e);
			// 二：强制刷新
			// this.$forceUpdate();
		}
    }
  }
</script>
```



## 日期或分钟或小时对不上

就是你选中一个时间，发现日期或者时间对不上选择的。这时候建议检查一下format属性，大概率是这个地方有问题