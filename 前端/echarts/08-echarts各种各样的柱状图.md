# echarts各种各样的柱状图



## 一：多系列柱状图

效果：

![05-各种各样柱状图1](./../../前端图片/echarts/05-各种各样柱状图1.png)



核心：

1. 就是要在`series`中写多个数值的配置而已很简单



### 代码示例：

```js
talentEducationBar() {
			let chart = this.$echarts.init(document.getElementById('talentEducation__bar'));
			let xData = [];
			let yData = [[], [], []];
			let nameData = ['博士', '硕士', '本科'];

			this.dataList.talentEducation.forEach(item => {
				xData.push(item.year + '年');

				yData[0].push(item.edu.doctor);
				yData[1].push(item.edu.master);
				yData[2].push(item.edu.undergraduate);
			});

			let option = {
				grid: {
					left: '10%',
					right: '5%',
					bottom: '10%'
				},
				legend: {
					left: 10,
					itemGap: 48,
					textStyle: {
						color: '#ffffff',
						fontSize: 12,
						fontFamily: 'Microsoft YaHei'
					},
					itemWidth: 14,
					itemHeight: 10,
					icon: 'rect'
				},
				xAxis: {
					type: 'category',
					axisTick: {show: false},
					data: xData,
					axisLine: {
						show: false,
						lineStyle: {
							type: 'dashed',
							color: '#fff',
							opacity: 0.3
						}
					}
				},
				yAxis: {
					type: 'value',
					splitLine: {
						show: true,
						lineStyle: {
							color: '#e8e8e8',
							opacity: 0.3,
							type: 'dashed'
						}
					},
					axisLabel: {
						color: '#fff',
						opacity: 0.5
					}
				},
				series: [
					{
						name: nameData[0],
						type: 'bar',
						barGap: 0.3,
						barCategoryGap: 40,
						data: yData[0],
						itemStyle: {
							borderRadius: [2, 2, 0, 0]
						}
					},
					{
						name: nameData[1],
						type: 'bar',
						barGap: 0.3,
						data: yData[1],
						itemStyle: {
							borderRadius: [2, 2, 0, 0]
						}
					},
					{
						name: nameData[2],
						type: 'bar',
						barGap: 0.3,
						data: yData[2],
						itemStyle: {
							borderRadius: [2, 2, 0, 0]
						}
					}
				],
				color: ['#006cff', '#ff9f7f', '#9fe6b8']
			};

			chart.setOption(option);
		}
```





## 二：堆叠柱状图

效果：

![04-各种各样的tooltip1](./../../前端图片/echarts/04-各种各样的tooltip1.png)



或者：

![05-各种各样柱状图2](./../../前端图片/echarts/05-各种各样柱状图2.png)



核心：

1. 还是要在series中写多个数据的配置，区别就是要加一个`stack: 'total'`属性
2. 下面那个x，y轴颠倒一下就可以了，配置type





### 代码示例

图一的代码：

```js
talentLevelBar() {
			let chart = this.$echarts.init(document.getElementById('talentLevel__bar'));
			let nameData = ['直列第一层次', '第一层次', '第二层次', '直列第三层次', '第三层次'];
			let xData = [];
			let yData = [[], [], [], [], []];

			// 填充数据
			this.dataList.talentLevel.forEach(item => {
				xData.push(item.year + '年');

				yData[0].push(item.level.lineFirst);
				yData[1].push(item.level.first);
				yData[2].push(item.level.second);
				yData[3].push(item.level.lineThird);
				yData[4].push(item.level.third);

				// let index = 0;
				// for (let key in item.level) {
				// 	if (!item.level.hasOwnProperty(key)) {
				// 		return; // 防止遍历到原型链上
				// 	}
				// 	// 如果不存在创建一个新的容器
				// 	yData[index] ? '' : (yData[index] = []);
				// 	yData[index].push(item.level[key]);
				// 	if (nameData.indexOf(nameMap[key]) === -1) {
				// 		nameData.push(nameMap[key]); // 添加名称
				// 	}
				// 	index++;
				// }
			});

			// 计算最上层label数据(把每一列数据加起来)
			let labelData = [];
			yData.forEach((item1, index1) => {
				item1.forEach((item2, index2) => {
					// 判断是否是number，如果不是，就先赋值成0再加，否则直接加
					typeof labelData[index2] == 'number'
						? (labelData[index2] += item2)
						: ((labelData[index2] = 0), (labelData[index2] += item2));
				});
			});

			let option = {
				grid: {
					top: '20%',
					left: '11%',
					right: '5%',
					bottom: '10%'
				},
				tooltip: {
					trigger: 'axis',
					axisPointer: {
						type: 'shadow'
					},
					backgroundColor: '#0f2159',
					borderColor: 'rgba(#00A6FF, 0.26)',
					position: 'left',
					formatter: function(params) {
						let str = '';
						params.forEach((item, index) => {
							let show = index === 0 ? '' : 'none';
							str += `<span style=\"color: #fff; font-size: 12px;\">${item.seriesName}:</span> <br /><span style=\"color:${item.color}; font-size: 12px;\">${item.value}</span> <br />`;
						});
						return str;
					}
				},
				legend: {
					left: 5,
					itemGap: 18,
					textStyle: {
						color: '#ffffff',
						fontSize: 12,
						fontFamily: 'Microsoft YaHei'
					},
					itemWidth: 14,
					itemHeight: 10,
					icon: 'rect'
				},
				xAxis: [
					{
						type: 'category',
						data: xData,
						axisTick: {show: false},
						axisLine: {show: false},
						axisLabel: {
							color: '#fff',
							margin: 12,
							fontFamily: 'Microsoft YaHei'
						}
					}
				],
				yAxis: [
					{
						type: 'value',
						splitLine: {
							show: true,
							lineStyle: {
								color: '#e8e8e8',
								opacity: 0.3,
								type: 'dashed'
							}
						},
						axisLabel: {
							color: '#fff',
							margin: 12,
							fontFamily: 'Roboto, Roboto-Regular',
							opacity: 0.5
						}
					}
				],
				series: [
					{
						name: nameData[0],
						type: 'bar',
						stack: 'total',

						data: yData[0]
					},
					{
						name: nameData[1],
						type: 'bar',
						stack: 'total',
						data: yData[1]
					},
					{
						name: nameData[2],
						type: 'bar',
						stack: 'total',
						data: yData[2]
					},
					{
						name: nameData[3],
						type: 'bar',
						stack: 'total',
						data: yData[3]
					},
					{
						name: nameData[4],
						type: 'bar',
						stack: 'total',
						label: {
							show: true,
							position: 'top',
							color: '#fff',
							fontFamily: 'Roboto, Roboto-Medium;',
							fontWeight: 500,
							formatter: params => {
								return `${this.formatNumber(labelData[params.dataIndex])}`;
							}
						},
						data: yData[4],
						barWidth: 26,
						itemStyle: {
							borderRadius: [4, 4, 0, 0]
						}
					}
				],
				color: ['#c54151', '#ff6e76', '#fddd60', '#7cffb2', '#05c091']
			};
			chart.setOption(option);
		}
```





## 三：圆柱的柱状图

这个可以去看同文件夹的，`04-echarts实现柱状图`