# axios 配置

一般项目都是两个环境，一个开发环境，一个生产环境。不同环境下的`api`不相同。所以我们可以进行配置



>第一步，在根目录（和package.json同级）下创建两个文件(vue-cli管理版本)
>
>`.env.development`和`.env.production`



`.env.development`的内容

```
ENV = 'development'

VUE_APP_BASE_API = '/api'
```

`.env.production`的内容

```
ENV = 'production'

VUE_APP_BASE_API = '/prod-api'
```



之后在分装`axios`的js文件中直接可以使用`VUE_APP_BASE_API`变量

```js
import axios from 'axios'
const service = axios.create({
  // 这里的baseURL可以直接使用定义在env上的变量
  baseURL: process.env.VUE_APP_BASE_API,
  timeout: 10000
  // headers: headers
})

export default service

```

