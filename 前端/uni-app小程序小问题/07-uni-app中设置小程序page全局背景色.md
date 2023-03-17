# uni-app中设置小程序page全局背景色

在根组件`App.vue`中的css中设置样式即可。



示例：

```vue
<script>
	export default {
		onLaunch: function() {
			console.warn('当前组件仅支持 uni_modules 目录结构 ，请升级 HBuilderX 到 3.1.0 版本以上！')
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import '@/uni_modules/uni-scss/index.scss';
	/* #ifndef APP-NVUE */
   /*设置page的全局样式*/
	page {
		background-color: #fff;
	}

	/* #endif */
	.example-info {
		font-size: 14px;
		color: #333;
		padding: 10px;
	}
</style>

```



## 单独设置

单独在一个页面的syle中设置page即可

示例：

```vue
...
<style>
  page {
    background: #eee;
  }
</style>
```



