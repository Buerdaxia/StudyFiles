# 自定义指令

```html

<div v-custom="{xxx}">
  
</div>
```

custom：指令名



## 一：自定义指令的定义语法

自定义指令总结：

一、定义语法：

(1)局部指令：

```vue
//方法一
// 指令比较复杂时候用方法一
new Vue({
	directives:{
		指令名:{配置对象}
	}
})

//方法二
new Vue({
	directives:{
		指令名:回调函数
	}
})
```

(2)全局指令：

```
//方法一
Vue.directive(指令名, 配置对象);
Vue.directive('big', {});
//方法二
Vue.directive(指令名, 回调函数)
Vue.directive('min', ()=>{})
```



三、备注：

1. 指令定义时不加v，但是使用时要加v-
2. 指令名如果是多个单词，要使用kebab-case命名方式(就是中间加-，big-number)，不要使用camelCase方式

## 二、**配置对象**中常用的几个回调：

（旧版的）

1）bind：指令与元素绑定时调用。

2）inserted(el, binding)：指令所在的元素被插入页面时调用

3）update：指令所在的模板结构被重新解构时调用

还有一个：

参数说明：

el：是绑定当前自定义指令的`dom`元素（例如：上面custom绑定的div）

binding：可以得到指令`=`后面跟的内容（例如：上面custom后面的`{xxx}`）通过`binding.value`



**新版本**

回调变成`2`个了:

1. `mounted(el, binding)`：在指令`dom`挂载时调用
2. `updated(el, binding)`：在指令绑定的`binding.value`改变时调用

传递的参数没变化



## 三：自定义指令传递参数

自定义指令也可以传递参数通过`:[参数名儿]`传递

访问：通过`binding.arg`来访问这个参数

示例：

```vue
<template>
	<p v-fsize:[unit]="fontSize">
    这是一个段落
  </p>
</template>
<script>
	export default {
    data() {
      fontSize: 18;
      unit: 'em'
    }
  }
</script>

<!--访问-->
Vue.directive('fsize', {
	mounted(el,binding) {
	return binding.art? el.style.fontSize = binding.value+binding.art: el.style.fontSize = binding.value + 'px';
}
});
```

