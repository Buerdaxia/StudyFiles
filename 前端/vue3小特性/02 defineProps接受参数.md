# 02 defineProps接受参数

该api是用来定义`props`的，vue2中父子组件传值不就是用的是`props`



## vue2中定义props

```vue

<script>
export default {
	props: ['foo'],
}
</script>

// 严格版，可以定义变量类型
<script>
export default {
	props: {
    foo: String
  },
}
</script>
```



## vue3中定义props

```vue
<template>
	<!-- 注意这里，可以直接使用，不需要props.foo-->	
  <div>
    {{foo}}
  </div>
</template>
// setup 语法糖，该script标签下所有内容都是setup函数的，并且无需return
<script setup>
import { defineProps } from 'vue'
const props = defineProps({
  foo: {
    type: String,
    required: true
  }
})

// 使用
props.foo
</script>
```

