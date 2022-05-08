# vue3，defineEmits的使用

vue2中我们又`this.$emit('xxx', xxx)`来帮我们触发自定义组件身上的绑定方法，vue3中使用的组合式API是`defineEmits`

> 引入并使用

父组件：

```vue
<template>
	<Child @f='function' />
</template>
```



子组件：

```vue
<script setup>
import { defineEmits } from 'vue'
// f是父组件给子组件绑定的自定义事件
const emits = defineEmits(['f'])
// 触发
emits('f')
</script>
```

