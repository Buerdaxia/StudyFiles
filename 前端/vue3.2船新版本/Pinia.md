# Pinia

建议直接官方文档，官方文档很细(●'◡'●)



**可以完美的和`vue3`的`composition API`结合使用**（太强大辣┭┮﹏┭┮）





## 核心步骤





## 一：创建store

示例：

>这里我们采用的和setup语法糖中一致的写法

```js
import {defineStore} from 'pinia';
import {ref, computed} from 'vue';
import axios from 'axios';

const API_URL = 'http://geek.itheima.net/v1_0/channels';

// 一般名称规范为use+名字+Store
export const useCounterStore = defineStore('counter', () => {
	// 定义数据（state）
	const count = ref(0);

	// 当以修改数据的方法（action）同步
	const increment = () => {
		count.value++;
	};

	// 定义一个getter
	const doubleCount = computed(() => count.value * 2);

	// 定义异步action
	const list = ref([]);
	const getList = async () => {
		const res = await axios.get(API_URL);
		list.value = res.data.data.channels;
	};

	// 以对象方式return 供组件使用
	return {count, doubleCount, list, increment, getList};
});

```





## 二：组件中使用



示例：

```vue
<script setup>
import {onMounted} from 'vue';
// 导入store方法
import {useCounterStore} from '@/stores/counter';
// 执行方法得到实例对象

const counterStore = useCounterStore();

onMounted(() => {
	// 调用store中的异步action
	counterStore.getList();
});

console.log('pinia', counterStore);
</script>

<template>
	<header>
		<div class="wrapper"></div>
	</header>

	<main>
		<div>
			<button @click="counterStore.increment">点我+1</button>
		</div>
		<div>计算值:{{ counterStore.count }}</div>
		<div>双倍值:{{ counterStore.doubleCount }}</div>

		<div>
			<ul>
				<!-- 获取list值，并直接渲染 -->
				<li v-for="item in counterStore.list" :key="item.id">
					{{ item.name }}
				</li>
			</ul>
		</div>
	</main>
</template>
```

