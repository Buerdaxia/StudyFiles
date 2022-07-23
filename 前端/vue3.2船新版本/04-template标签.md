# template标签

template标签，渲染的时候不会被渲染到页面上，就和react中的fragment标签一致。

示例：

```vue
<template v-if="condition">
	<h1>
    我是标题嘻嘻嘻
  </h1>
</template>


<template v-for="todo in todos">
	<h1>
    {{todo.content}}
  </h1>
</template>
```



常常在一些特殊特定情况和v-if于v-for一起使用。

