# v-if与v-for循环的问题

首先`v-if`和`v-for`不能放在同一个标签上使用。

>原因：v-for的优先级高于v-if，当丢在同一个标签时，每一个if都会先循环一遍，再进行判断。会极大的影响到性能

## 解决办法一：(判断条件在循环外部)

将`v-if`和`v-for`分开再不同的标签上,一般会选择再要进行的循环身上套上一个`template`标签(因为template标签不会真正渲染dom)，里面填入`v-if`内容

```html
<template v-if="isShow">
  <div v-for="item in arr" :key="item.id">
    {{item}}
  </div>
</template>
```



## 解决办法二：（判断条件在循环内部）

如果判断是否显示的`isShow`在循环内部，我们可以使用计算属性`computed`来过滤掉不显示的内容

```js
computed: {
	item: function() {
		return this.list.filter((item) => {
      // 返回isShow = true的项，组成一个新数组
			return item.isShow;
		})
	}
}
```

