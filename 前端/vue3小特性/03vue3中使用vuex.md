# vue3中使用vuex

vue3中使用vuex需要引入一个方法名字叫做`useStore`

使用步骤

>第一步：在<script>标签中映入useStore，并获得对象

```vue
<script setup>
import { ref, reactive } from 'vue'
import { useStore } from 'vuex'
const store = useStore()
</script>
```

>第二步：就和vue2中的vuex使用一致了

```js
dispath.('action中方法名',数据);
store.commit.('mutation中方法名',数据)
```

