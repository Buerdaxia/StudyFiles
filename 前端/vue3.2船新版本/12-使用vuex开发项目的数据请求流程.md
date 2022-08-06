# 使用vuex开发项目的数据请求流程

## **第一步：在`src/apis`文件夹中定义好请求后台的方法**

示例：

```js
//auth.js
import { request } from '../utils/request';

// 读出jwt
export function getJwtToken() {
	const token = window.localStorage.getItem('jwtToken');
	return token;
}

// 写入jwt
export function setJwtToken(jwt) {
	localStorage.setItem('jwtToken', jwt);
}

export function saveUser(user) {
	localStorage.setItem('user', JSON.stringify(user));
}
export function getUser(user) {
	return JSON.parse(localStorage.getItem('user'));
}

// 核心注册函数
export async function register(email, username, password) {
	const result = await request('/api/auth/local/register', {
		method: 'POST',
		auth: false,
		body: {
			email,
			username,
			password,
			name: username
		}
	});

	setJwtToken(result.jwt);
	saveUser(result.user);

	return result.user;
}

```







## **第二步：在vuex中定义actions来调用第一步的方法。**

1）定义一个state

2）定义一个修改state的mutations

3）定一个actions触发请求数据函数，然后commit触发mutations



核心：**就是原来在各个组件自身身上存储的状态，都存储到对应的vuex中的state里了，请求数据的方法也都放在vuex的actions中**



在模块`user`中写入：

```js
// user/index.js
import { getUser, register } from '../../apis/auth';

export const user = {
	state() {
		return {
			user: getUser() || {}
		};
	},
	mutations: {
		setUser(state, user) {
			state.user = user;
		}
	},
	actions: {
		// 注意这里不要和apis中的方法重名，会有循环调用错误出现
		async registerUser({ commit }, { email, username, password }) {
			const user = await register(email, username, password);

			// 调用commit保存user
			commit('setUser', user);
		}
	}
};

```



## **第三步：再到组件中去触发actions来获取数据**

1）触发actions

2）从vuex中的state身上读取数据



核心：**绑定vuex中state上存储的数据，触发actions方法来修改数据或请求数据**

在`login.vue`中

```vue
<template>
	<div class="loginContainer">
		<img :src="phone" alt="图片" />
		<div class="loginForm">
			<img :src="logo" alt="logo" class="logo" />
			<form @submit.prevent>
				<input type="text" placeholder="邮箱" v-model="email" />
				<input v-if="!hasUser" type="text" placeholder="用户名" v-model="username" />
				<input type="password" placeholder="密码" v-model="password" />
				<TheButton type="submit" class="loginButton" @click="register">{{
					hasUser ? '登录' : '注册'
				}}</TheButton>
			</form>

			<p class="register" @click="hasUser = !hasUser">
				{{ hasUser ? '还没有账号? 点击注册' : '已有账号? 点击登录' }}
			</p>

			<div v-if="!hasUser" class="agreement">
				<input type="checkbox" v-model="agreementChecked" /> 勾选表示同意隐私协议和用户规范
			</div>
		</div>
	</div>
</template>

<script setup>
import logo from '../assets/logo.svg';
import phone from '../assets/phone.png';
import TheButton from '../components/TheButton.vue';
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';

let hasUser = ref(true);
const email = ref('');
const username = ref('');
const password = ref('');
const agreementChecked = ref(false);
const store = useStore();
const router = useRouter();

// 注册函数
async function register() {
	if (!agreementChecked.value) {
		alert('请先勾选用户协议');
		return;
	}

	// 触发user模块中的action
	await store.dispatch('registerUser', {
		email: email.value,
		username: username.value,
		password: password.value
	});
	// 使用replace的目的是防止用户点退回退回到
	router.replace('/');
}
</script>
```



**注意：建议不要再vuex中控制路由，而是在组件自身控制，如果在vuex中控制路由，会导致vuex和router相互绑定，不易维护**