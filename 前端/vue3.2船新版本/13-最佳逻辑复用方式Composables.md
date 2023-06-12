# 最佳逻辑复用方式`Composables`

其实：像下面的`ref`呀、`watch`呀都是`composables`。

无敌啊这个：直接能把逻辑抽离出来，无敌了。

替代`vue2`中的`Mixin`但是比`Mixin`强多了

* `composable`是一个普通的js函数
* setup()中的代码全部都可以在`composables`中编写
* `composables`**逻辑越独立越好**
* 能减少组件文件的代码，增强复用性



## 几个注意点：

## 1.命名

组合式函数约定用驼峰命名法命名，并以“use”作为开头。

##  2.输入值

尽管其响应性不依赖 ref，组合式函数仍可接收 ref 参数。如果编写的组合式函数会被其他开发者使用，**你最好在处理输入参数时兼容 ref 而不只是原始的值**。`unref()`工具函数会对此非常有帮助：

```js
import { unref } from 'vue'

function useFeature(maybeRef) {
// 若 maybeRef 确实是一个 ref，它的 .value 会被返回
// 否则，maybeRef 会被原样返回
const value = unref(maybeRef)
}
```



如果你的组合式函数在接收 ref 为参数时会产生响应式 effect，请确保使用 `watch()` 显式地监听此 ref，或者在 `watchEffect()` 中调用 `unref()` 来进行正确的追踪。

## 3.返回值

你可能已经注意到了，我们一直在组合式函数中使用 `ref()` 而不是 `reactive()`。我们推荐的约定是组合式函数始终返回一个包含多个 ref 的普通的非响应式对象，这样该对象在组件中被解构为 ref 之后仍可以保持响应性：

```js
// x 和 y 是两个 ref
const { x, y } = useMouse()
```

从组合式函数返回一个响应式对象会导致在对象解构过程中丢失与组合式函数内状态的响应性连接。与之相反，ref 则可以维持这一响应性连接。





## 示例：

现在有一个`category`组件，组件包含2个逻辑内容，一：获取banner，二：获取组件分类。



```vue
<script setup>
import {onMounted, ref} from 'vue';
import {useRoute, onBeforeRouteUpdate} from 'vue-router';
import {getCategoryAPI} from '@/apis/category.js';
import {getBannerAPI} from '@/apis/home.js';
import GoodsItem from '../Home/components/GoodsItem.vue';
	
  // banner和cate的响应式数据
 	const bannerList = ref([]);
	const categoryData = ref({});
	// 获取route对象
  const route = useRoute();
  
  // 获取banner
	const getBanner = async () => {
		const {data: res} = await getBannerAPI({distributionSite: '2'});
		bannerList.value = res.result;
	};
  
  // 获取分类
	const getCateGory = async (params = route.params) => {
		const {data: res} = await getCategoryAPI(params.id);
		categoryData.value = res.result;
	};

	onBeforeRouteUpdate(to => {
		getCateGory(to.params);
	});
  
  // 生命周期获取内容
	onMounted(() => {
    getCateGory();
		getBanner();
	});
</script>

<template>
	<div class="top-category">
		<div class="container m-top-20">
			<!-- 面包屑 -->
			<div class="bread-container">
				<el-breadcrumb separator=">">
					<el-breadcrumb-item :to="{path: '/'}">首页</el-breadcrumb-item>
					<el-breadcrumb-item>{{ categoryData.name }} </el-breadcrumb-item>
				</el-breadcrumb>
			</div>
			<!-- 轮播图 -->
			<div class="home-banner">
				<el-carousel height="500px">
					<el-carousel-item v-for="item in bannerList" :key="item.id">
						<img :src="item.imgUrl" alt="" />
					</el-carousel-item>
				</el-carousel>
			</div>

			<div class="sub-list">
				<h3>全部分类</h3>
				<ul>
					<li v-for="i in categoryData.children" :key="i.id">
						<RouterLink to="/">
							<img :src="i.picture" alt="" />
							<p>{{ i.name }}</p>
						</RouterLink>
					</li>
				</ul>
			</div>
			<div class="ref-goods" v-for="item in categoryData.children" :key="item.id">
				<div class="head">
					<h3>- {{ item.name }}-</h3>
				</div>
				<div class="body">
					<GoodsItem v-for="good in item.goods" :good="good" :key="good.id" />
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.home-banner {
	width: 1240px;
	height: 500px;
	margin: 0 auto;
	img {
		width: 100%;
		height: 500px;
	}
}

.top-category {
	h3 {
		font-size: 28px;
		color: #666;
		font-weight: normal;
		text-align: center;
		line-height: 100px;
	}

	.sub-list {
		margin-top: 20px;
		background-color: #fff;

		ul {
			display: flex;
			padding: 0 32px;
			flex-wrap: wrap;

			li {
				width: 168px;
				height: 160px;

				a {
					text-align: center;
					display: block;
					font-size: 16px;

					img {
						width: 100px;
						height: 100px;
					}

					p {
						line-height: 40px;
					}

					&:hover {
						color: $xtxColor;
					}
				}
			}
		}
	}

	.ref-goods {
		background-color: #fff;
		margin-top: 20px;
		position: relative;

		.head {
			.xtx-more {
				position: absolute;
				top: 20px;
				right: 20px;
			}

			.tag {
				text-align: center;
				color: #999;
				font-size: 20px;
				position: relative;
				top: -20px;
			}
		}

		.body {
			display: flex;
			justify-content: space-around;
			padding: 0 40px 30px;
		}
	}

	.bread-container {
		padding: 25px 0;
	}
}
</style>

```



注意：**会发现这两个模块逻辑相互独立，互相没有干扰，这样我们就可以抽离一下逻辑**



抽离逻辑版本：

严格按照上面三点来，抽离两个`composables`：

```js
// composables/useBanner.js
// 封装banner轮播相关业务代码
import {onMounted, ref} from 'vue';
import {getBannerAPI} from '@/apis/home.js';

export function useBanner() {
	const bannerList = ref([]);
	// 获取banner
	const getBanner = async () => {
		const {data: res} = await getBannerAPI({distributionSite: '2'});
		bannerList.value = res.result;
	};

	onMounted(() => {
		getBanner();
	});

	return {
		bannerList
	};
}

```



```js
// composables/useCategory.js
// 分装分类数据业务相关代码
import {onMounted, ref} from 'vue';
import {useRoute, onBeforeRouteUpdate} from 'vue-router';
import {getCategoryAPI} from '@/apis/category.js';
export function useCategory() {
	const categoryData = ref({});
	const route = useRoute();
	const getCateGory = async (params = route.params) => {
		const {data: res} = await getCategoryAPI(params.id);

		categoryData.value = res.result;
	};

	onBeforeRouteUpdate(to => {
		getCateGory(to.params);
	});

	onMounted(() => {
		getCateGory();
	});

	return {
		categoryData
	};
}
```



最后**在组件中调用函数**获取数据或者方法组合回来使用：

```vue
<script setup>
// 引入两个composables
import {useBanner} from './composables/useBanner.js';
import {useCategory} from './composables/useCategory.js';
import GoodsItem from '../Home/components/GoodsItem.vue';

// 直接解构出来
const {bannerList} = useBanner();
const {categoryData} = useCategory();
</script>

<template>
	<div class="top-category">
		<div class="container m-top-20">
			<!-- 面包屑 -->
			<div class="bread-container">
				<el-breadcrumb separator=">">
					<el-breadcrumb-item :to="{path: '/'}">首页</el-breadcrumb-item>
					<el-breadcrumb-item>{{ categoryData.name }} </el-breadcrumb-item>
				</el-breadcrumb>
			</div>
			<!-- 轮播图 -->
			<div class="home-banner">
				<el-carousel height="500px">
					<el-carousel-item v-for="item in bannerList" :key="item.id">
						<img :src="item.imgUrl" alt="" />
					</el-carousel-item>
				</el-carousel>
			</div>

			<div class="sub-list">
				<h3>全部分类</h3>
				<ul>
					<li v-for="i in categoryData.children" :key="i.id">
						<RouterLink to="/">
							<img :src="i.picture" alt="" />
							<p>{{ i.name }}</p>
						</RouterLink>
					</li>
				</ul>
			</div>
			<div class="ref-goods" v-for="item in categoryData.children" :key="item.id">
				<div class="head">
					<h3>- {{ item.name }}-</h3>
				</div>
				<div class="body">
					<GoodsItem v-for="good in item.goods" :good="good" :key="good.id" />
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.home-banner {
	width: 1240px;
	height: 500px;
	margin: 0 auto;
	img {
		width: 100%;
		height: 500px;
	}
}

.top-category {
	h3 {
		font-size: 28px;
		color: #666;
		font-weight: normal;
		text-align: center;
		line-height: 100px;
	}

	.sub-list {
		margin-top: 20px;
		background-color: #fff;

		ul {
			display: flex;
			padding: 0 32px;
			flex-wrap: wrap;

			li {
				width: 168px;
				height: 160px;

				a {
					text-align: center;
					display: block;
					font-size: 16px;

					img {
						width: 100px;
						height: 100px;
					}

					p {
						line-height: 40px;
					}

					&:hover {
						color: $xtxColor;
					}
				}
			}
		}
	}

	.ref-goods {
		background-color: #fff;
		margin-top: 20px;
		position: relative;

		.head {
			.xtx-more {
				position: absolute;
				top: 20px;
				right: 20px;
			}

			.tag {
				text-align: center;
				color: #999;
				font-size: 20px;
				position: relative;
				top: -20px;
			}
		}

		.body {
			display: flex;
			justify-content: space-around;
			padding: 0 40px 30px;
		}
	}

	.bread-container {
		padding: 25px 0;
	}
}
</style>

```

