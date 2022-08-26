# element-ui按需导入

所有的操作官网上都有，我这里只写一下main.js入口文件中的引入方式。

当前面所有准备工作完毕之后，一般会在src下创建一个plugins/element.js文件，里面写着引入的组件，之后再引入回main.js入口文件。



示例：

```js
// src/plugins/element.js
import { Button, Select } from 'element-ui';
Vue.use(Button)
Vue.use(Select)
```



在main.js中引入element.js文件：

```js
// main.js
import './plugins/element.js'
...
```

