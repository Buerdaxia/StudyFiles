# vue3中的route和router

由于vue3组合式api的原因，就是用什么就要调用相应的函数来调用一下，不再像vue2那样



## vue2中使用route，router

vue2中只要将vue-router引入并使用，并将router挂载到App身上，就可以直接通过`this.$route`和`this.$router`直接使用

>挂载好vue-router

```vue
new Vue({
	router,
	store,
	render: h => h(App)
}).$mount('#app');
```

>使用 `this.$router.push`

```vue
<script>
// 登录
		login() {
			this.$refs.loginForm.validate(async valid => {
				if (!valid) {
					return false;
				}
				const { data: res } = await this.$http({
					url: '/users/login',
					method: 'POST',
					data: this.loginForm
				});
				if (res.code !== 0) {
					this.$message.error(res.message);
					return;
				}
				this.$message.loading({ content: '登陆中...', key: 'logined' });

				sessionStorage.setItem('isNew', res.result.isNew);
				sessionStorage.setItem('role', res.result.role);
				sessionStorage.setItem('username', res.result.username);
				sessionStorage.setItem('token', res.result.token);
				await this.getMenuItem();
				initRouter();
				setTimeout(() => {
					this.$message.success({ content: res.message, key: 'logined' });
          // 像这里直接就使用了
					this.$router.push({
						path: '/home'
					});
				}, 2000);
			});
		},
</script>
```



## vue3中使用route和router

vue3中使用route和router需要引入两个`api`和使用`vuex`方式一致，引入并调用之后，就可以直接使用

>引入`useRoute`和`useRouter`并调用获得对象

```vue
<script setup>
import { useRoute, useRouter } from 'vue-router'
const route = useRoute()
const router = useRouter()
</script>
```

>使用

```vue
<script>
import { ref, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
const route = useRoute()
const router = useRouter()

const breadcrumbList = ref([])
const initBreadcrumbList = () => {
  breadcrumbList.value = route.matched
}

/**
 * @param {path} 跳转路径
 */
const handleRedirect = path => {
  router.push(path)
}

/**
 *  @param {route} 路由
 *  @param {函数} 修改函数
 *  @param {配置对象} watch的配置对象
 *  @description watch函数的用法
 */
watch(
  route,
  () => {
    initBreadcrumbList()
  },
  { deep: true, immediate: true }
)
</script>
```

