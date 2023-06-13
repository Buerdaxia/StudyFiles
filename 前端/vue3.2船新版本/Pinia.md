# Pinia

建议直接官方文档，官方文档很细(●'◡'●)

https://pinia.vuejs.org/zh/



1. state
2. getter
3. action（pinia中去除了vuex中的mutations，并且action既可以同步也能够异步）



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



## 方法

## `storeToRefs()`

作用：这个方法和`vue3`中的`toRefs()`功能是一致的，都是为了方便解构操作还能不丢失响应性



示例：

```vue
<script setup>
import {onMounted} from 'vue';
// 导入store方法
import {useCounterStore} from '@/stores/counter';

// 从pinia中引入方法
import {storeToRefs} from 'pinia';
  
// 执行方法得到实例对象
const counterStore = useCounterStore();

// 直接结构赋值 (会有响应式丢失的问题)
// const {count, doubleCount} = counterStore;

// 保持数据响应式 (使用storeToRefs)
const {count, doubleCount} = storeToRefs(counterStore);

// 注意：方法，从原本store对象中结构出来即可
const {increment} = counterStore;

  
  
  
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
			<button @click="increment">点我+1</button>
		</div>
		<div>计算值:{{ count }}</div>
		<div>双倍值:{{ doubleCount }}</div>

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

<style scoped>
header {
	line-height: 1.5;
}

.logo {
	display: block;
	margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
	header {
		display: flex;
		place-items: center;
		padding-right: calc(var(--section-gap) / 2);
	}

	.logo {
		margin: 0 2rem 0 0;
	}

	header .wrapper {
		display: flex;
		place-items: flex-start;
		flex-wrap: wrap;
	}
}
</style>

```







## 使用遵循原理

遵循理念：**和数据相关的所有操作（state, action）都要放到Pinia中，组件只负责触发action函数**



>注意：action就是方法也有可能是调用接口的方法，because pinia中的action是可以异步的





# 持久化插件(pinia-plugin-persistedstate)

可以直接去官网查看：https://prazdevs.github.io/pinia-plugin-persistedstate/

提供了很多配置项，很好使用。



运行机制：设置store时，会自动把数据同步进localStorage，取数据时优先从localStorage中获取。

注意：**这里是每一次修改都会写进localStorage，每一次！每一次！！每一次！！！**

所以像是我们退出登录想要把localStorage的用户信息清除掉的操作，我们只需要初始化赋值一下就完事儿了

示例：

```js
// stores/user.js 的一部分内容
// 退出时清除用户信息
const clearUserInfo = () => {
  // 直接恢复初始化
  userInfo.value = {};
  // 注意这里可以不用下面这一步，因为用了持久化插件，始终会在localStorage中保存最新赋值，所以直接初始化一下就没了
  // window.localStorage.removeItem('user');
};
```

