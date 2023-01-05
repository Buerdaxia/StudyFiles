# echarts图表响应式

echarts为了帮助开发人员解决响应式问题，有一个专门的方法，叫做`resize`方法

语法：

```js
const myChart = echart.init(...);
window.onresize = () => {
  myChart.resize();
};                 
```



使用：

我一般都是用vue进行开发的，所以这里是在vue项目中的基本用法：

```vue
<script>
	export default {
    data() {
      return {
        myChart: ''
      }
    },
    mounted() {
      // 都是在dom渲染结束的声明周期中监听事件
      window.onresize = () => {
        this.myChart.resize();
        // 如果有多个需要将每一个echart来进行resize
        this.myChart2.resize();
        ...
      }; 
    },
    methods: {
      chartInit() {
        this.myChart = echart.init(...);
        this.myChart.setOption({....});
      }
    }
  }
</script>
```

