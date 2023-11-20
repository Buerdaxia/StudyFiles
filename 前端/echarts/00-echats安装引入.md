# echats安装引入

这些内容官网上面都有介绍，这里简单复述一下



## 安装

我们这里采用npm安装，还有很多其他安装方式，详情请见官网

```
npm install echarts
```





## 在vue2中引入

>注意：
>
>下面引入方式是5.0版本及以上的方式

第一步：创建：`src/plugins/echats.js`

```js
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import * as echarts from 'echarts/core';
// 引入柱状图图表，图表后缀都为 Chart
import {BarChart, PieChart} from 'echarts/charts';
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
	TitleComponent,
	TooltipComponent,
	GridComponent,
	DatasetComponent,
	TransformComponent
} from 'echarts/components';
// 标签自动布局、全局过渡动画等特性
import {LabelLayout, UniversalTransition} from 'echarts/features';
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import {CanvasRenderer} from 'echarts/renderers';

// 注册必须的组件
echarts.use([
	PieChart,
	TitleComponent,
	TooltipComponent,
	GridComponent,
	DatasetComponent,
	TransformComponent,
	BarChart,
	LabelLayout,
	UniversalTransition,
	CanvasRenderer
]);

echarts.registerTheme('myTheme', {
	animationDuration: function(idx) {
		// 越往后的数据时长越大
		return 2000;
	},
	tooltip: {}
});

export default echarts;

```



第二步：将echarts放入vue原型链中

`main.js`

```js
import Vue from 'vue';
import App from './App';
import echarts from '@/plugins/echarts';
Vue.prototype.$echarts = echarts; // echarts
...
```



