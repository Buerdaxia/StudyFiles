# 封装breadCrumb面包屑组件

前置条件：

我这里使用的就是element-ui的面包屑组件

具体组件代码：

```vue
<el-breadcrumb separator-class="el-icon-arrow-right">
  <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
  <el-breadcrumb-item>活动管理</el-breadcrumb-item>
  <el-breadcrumb-item>活动列表</el-breadcrumb-item>
  <el-breadcrumb-item>活动详情</el-breadcrumb-item>
</el-breadcrumb>
```





## 方式一：使用$route.matched属性

具体操作方式：

1. **要按照Vue Router规范来设计多级路由表(树状的多级路由)**，meta属性中放置面包屑名称
2. 面包屑组件中通过监听$route变化，来获取$route.matched属性，该属性会返回到当前路由页面的所有路由，以数组方式返回
3. 直接渲染$route.matched属性返回的数据即可



具体效果：

![bread-01](./assets/bread-01.gif)

### 实现代码

这里只展示些核心的封装代码



面包屑封装组件：

```vue
<template>
	<div class="col-xs-offset-2 col-xs-8" style="padding: 20px 0;">
		<el-breadcrumb separator="/">
			<el-breadcrumb-item
				v-for="(item, index) in breadcrumbData"
				:key="index"
				:to="index === breadcrumbData.length - 1 ? '' : item"
				>{{ item.meta }}</el-breadcrumb-item
			>
      <!-- 要注意以下这里的to属性，这样写的目的就是让最后一级不进行跳转-->
		</el-breadcrumb>
	</div>
</template>
<script>
export default {
	name: 'breadCrumb',
	data() {
		return {
			breadcrumbData: []
		};
	},
	watch: {
		$route: {
			handler() {
				this.breadcrumbData = this.$route.matched ? this.$route.matched : [];
				// console.log(this.$route.matched);
			},
			immediate: true
		}
	}
};
</script>

```





### 优缺点

**优点：**

1. 实现起来简单，路由表也符合设计规范，也最方便
2. 能够适应多种情景，无论是刷新页面，还是直接访问url都能保持面包屑一致
3. 能够保存路由参数，query和定义的路径参数都能够保存下来，上级跳转也非常完美



**缺点：**

1. 要求存在可以手写的路由表，自己控制meta属性，其他的一些工具可能生成的路由表不易设置meta属性
2. 路由表必须保证要有清晰的树状结构，如果路由的结构不清晰，或者不方便设置为树形，或者是网状结构九不能使用该方法



**如果是嵌套路由，该方法是面包屑的首选方式**





## 方式二：构建新路由表，反向查找

具体操作方式：

1. 要自定义一张路由表，表中的内容，要符合你自己的面包屑一致，还必须要和路由表中的name属性，和meta属性进行关联
2. 监听$route，跳转时将name属性和自定义的路由表进行比对，然后将面包屑的完整路径拼接出来即可





### 实现代码

面包屑封装组件：

```vue
<template>
	<div class="col-xs-offset-2 col-xs-8" style="padding: 20px 0;">
		<el-breadcrumb separator="/">
			<el-breadcrumb-item
				v-for="(item, index) in breadcrumbData"
				:key="index"
				:to="index === breadcrumbData.length - 1 ? '' : item"
				>{{ item.meta }}</el-breadcrumb-item
			>
		</el-breadcrumb>
	</div>
</template>
<script>
export default {
	name: 'breadCrumb',
	data() {
		return {
			temp: [],
			breadcrumbData: [],
      
      // 建立一张路由表
			breadcrumbTree: [
				{
					name: 'about',

					meta: 'about页'
				},
				{
					name: 'home',
					meta: 'home页',
					// 子路由
					children: [
						{
							meta: 'news页',
							name: 'news'
						},
						{
							name: 'message',
							meta: 'message页',
							children: [
								{
									name: 'detail',
									meta: 'detail页'
								}
							]
						}
					]
				}
			]
		};
	},
	watch: {
		$route: {
			handler() {
				// this.breadcrumbData = this.$route.matched ? this.$route.matched : [];
				// console.log(this.$route.matched);
				this.temp = [];
				this.formatBreadCrumbData(this.breadcrumbTree, this.$route.name);
			},
			immediate: true
		}
	},
	methods: {
		formatBreadCrumbData(arr, target) {
			if (!arr || !arr.length) {
				return;
			}

			for (let i = 0; i < arr.length; i++) {
				const item = arr[i];
				this.temp.push(item);
				console.log(111, this.temp);
				if (item.name === target) {
					this.breadcrumbData = JSON.parse(JSON.stringify(this.temp));
					// finalData = JSON.parse(JSON.stringify(item));
					break;
				}

				// 如果不是对应的，就向下查找
				this.formatBreadCrumbData(item.children, target);
				// 如果向下查找没找到，就将data弹出
				this.temp.pop();
			}
		}
	}
};
</script>

```



### 优缺点

**优点：**

1. 可以自己组织面包屑层级名称，即使不对应路由表的结构



**缺点：**

1. 不能传递路由参数







## 方式三：利用vuex+localstorage