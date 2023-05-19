# echarts图表响应式

echarts为了帮助开发人员解决响应式问题，有一个专门的方法，叫做`resize`方法

语法：

```js
const myChart = echart.init(...);
window.onresize = () => {
  myChart.resize();
}; // 这样写监听事件，会被覆盖


// 注意，封装组件时，建议使用下面这种，应为addEventListener不会覆盖事件，调用一个监听一个，会将所有事件都监听
window.addEventListener('resize', () => {
  chart.resize();
}); 
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
      // 上面的改进版
      window.addEventListener('resize', () => {
        myChart.resize();
      }); 
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





## 组件封装

简单封装

```vue
<template>
	<div class="qChart" ref="qChart"></div>
</template>

<script>
import echarts from 'echarts';
export default {
	props: {
		options: {
			type: Object,
			default: () => {}
		}
	},
	data() {
		return {};
	},
	mounted() {
		this.init();
	},
	methods: {
		init() {
			let dom = this.$refs.qChart; // 这里最好用this.$refs来获取dom，否则其他方式会有bug
			let chart = echarts.init(dom);
			chart.setOption(this.options);
			window.addEventListener('resize', () => {
				chart.resize();
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.qChart {
	width: 100%;
	height: 100%;
}
</style>

```



使用：

```vue
<template>
	<div>
    <q-chart ref="chart" :options="levelChartOptions"></q-chart>
  </div>
</template>

<script>
export default {
  data() {
    return {
      levelChartOptions: ''
    }
  },
  methods: {
    getData() {
      this.$http('...').then((res) => {
        let options = xxxx;
        this.levelChartOptions = options;
        this.$nextTick(() => {
          this.$refs.chart.init(); // 注意这里一定要这样调用一下
        })
      })
    }
  }
}

</script>
```

