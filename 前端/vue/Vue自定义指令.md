# 自定义指令

```html

<div v-custom="{xxx}">
  
</div>
```

custom：指令名



## 自定义指令

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

二、**配置对象**中常用的三个回调：

1）bind：指令与元素绑定时调用。

2）inserted(el, binding)：指令所在的元素被插入页面时调用

3）update：指令所在的模板结构被重新解构时调用

参数说明：

el：是绑定当前自定义指令的元素（例如：上面custom绑定的div）

binding：可以得到指令`=`后面跟的内容（例如：上面custom后面的`{xxx}`）

三、备注：

1. 指令定义时不加v，但是使用时要加v-
2. 指令名如果是多个单词，要使用kebab-case命名方式(就是中间加-，big-number)，不要使用camelCase方式