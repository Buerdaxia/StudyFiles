# avatar组件(头像组件问题)

`<el-avatar icon="el-icon-user-solid"></el-avatar>`

avatar组件有在引入`src`时有一个小问题，就是不能直接引入相对路径的图片

例如:

```
<el-avatar src="../assets/avatar.jpg"></el-avatar>
```

会导致图片加载不出来。



## 解决方式

1. 使用绝对路径（不推荐）
2. 使用import（推荐使用该方式，符合es6语法）

```vue
<template>
<el-avatar :src="avatarUrl"></el-avatar>
</template>
<script>
import jgpurl from '@/assets/userAvatar.jpg'
export default {
  name: 'Home',
  data() {
    return {
      // 头像图片地址
      avatarUrl: jgpurl
    }
  }
}
</script>
```

3. 使用require

```vue
<template>
<el-avatar :src="avatarUrl"></el-avatar>
</template>
<script>
export default {
  name: 'Home',
  data() {
    return {
      // 头像图片地址
      avatarUrl: requite('../assets/avatar.jpg')
    }
  }
}
</script>
```