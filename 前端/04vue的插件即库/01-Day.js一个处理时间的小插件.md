# Day.js一个处理时间的小插件

我们需要格式化日期是，就可以使用改插件来帮助我们

> 安装

```
npm install Day.js
```

> 使用

创建`src/utils/filters.js`

```js
const dayjs = require('dayjs')

/**
 *
 * @param {val} 必传参数 时间戳
 * @param {format } 可选参数，时间格式
 */
const filterTimes = (val, format = 'YYYY-MM-DD') => {
  if (isNull(val)) {
    return '---'
  } else {
    val = parseInt(val) * 1000
    return dayjs(val).format(format)
  }
}

// 判空函数
/**
 *
 * @param {date} 需要判空的变量
 */
export const isNull = date => {
  if (!date) return true
  if (JSON.stringify(date) === '{}') return true
  if (JSON.stringify(date) === '[]') return true
}

// 默认暴露一个添加方法的函数，到main.js中引入并调用
export default app => {
  // 将filterTimes添加到app身上
  app.config.globalProperties.$filters = {
    filterTimes
  }
  // vue2 可以写为 app.prototype.$filters = {filterTimes}
}

```



> 在main.js引入并使用

```js

// 引入filter
import filters from '@/utils/filters.js'
// 注意这个函数一定要放在 createApp下面 否则app访问不到
filters(app)
```



>之后就可以在组件中愉快的使用辣

`$filters.filterTimes(日期参数)`