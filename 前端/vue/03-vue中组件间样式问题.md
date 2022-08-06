# `vue`中组件间样式问题

## 全局样式

全局样式一般有下面这几种

1. 在`App.vue`中的<style>标签（不带scoped属性）中书写样式，是作用于全局的。

2. 创建`css`文件，并引入到`App`组件中



## 局部样式

在一个组件的<style>标签中添加scoped属性后，该标签下的所有属性都将只在自己组件内生效。





## `lang`属性

如果想要使用一些预编译器例如less，scss就需要添加一个lang属性，并下载对应的包

在`vite`环境下使用`scss`:

>第一步：安装

```
yarn add scss
```

>第二步：使用

```vue
<style lang='scss' scoped>
xxx
</style>
```





## :deep()

开启`scoped`的组件，`css`样式都作用自身。但是现在父组件想修改子组件的样式怎么办嘞？

首先：默认情况下，即使子组件有`scoped`属性，**父组件也能修改和访问到子组件根元素的选择器**

示例：

```vue
<!-- 父组件-->
<template>
	<Item></Item>
</template>
<style>
  /*这里可以访问到子组件的.text类选择器*/
  .text {
    font-size: 20px;
    color: red;
  }
</style>

<!--子组件-->
<template>
	<div class='text'></div>
</template>


```



第二：如果不是子元素的跟组件该怎么修改样式呢？

这时候就需要`:deep`深度选择器了

示例：我们要在父组件中修改子组件的`a`标签样式

```vue
<!-- 父组件-->
<template>
	<Item></Item>
</template>
<style>
  /*通过:deep访问到子组件的a属性*/
  .text :deep(a) {
    font-size: 20px;
    color: red;
  }
</style>

<!--子组件-->
<template>
	<div class='text'>
    <a>我是a标签</a>
  </div>
</template>


```



## :sloted()

当子组件中使用了slot插槽时，想在子组件中修改父组件中传递的HTML标签时，就可以使用`sloted`选择器。

示例：

```vue
<!--父组件-->
<template>
	<div>
   <Item>
     <p>
       我是子组件中slot的内容
  	</p>
  </Item>
  </div>
</template>


<!--子组件Item-->
<template>
	<div>
   	<slot />
  </div>
</template>
<style>
  /* 修改父组件中的p*/
  :sloted(p) {
    font-size:20px;
    color: red;
  }
</style>
```



## vue3.2样式绑定响应式数据

在`vue3.2`中，css也能访问data上的变量了，从而形成一种响应式的css。

核心：通过`v-bind()`函数。

示例：

```vue
<template>
	<div>
		<div class="box"></div>
		<div class="control">
			<input type="range" min="0" max="360" v-model="degree" />
		</div>
		<p>当前角度{{ degree }}</p>
	</div>
</template>
<script>
export default {
	data() {
		return {
			degree: 0
		};
	}
};
</script>
<style scoped>
.box {
	width: 250px;
	height: 250px;
	border-radius: 8px;
	background-color: hsl(280deg, 100%, 60%);
  /*这里直接调用v-bind函数访问到data中的degree*/
	transform: rotate(v-bind(degree + 'deg'));
}
.control {
	margin-top: 64px;
}
</style>
```

