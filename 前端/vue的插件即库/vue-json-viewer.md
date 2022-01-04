# vue-json-viewer

简单易用的json内容展示组件，支持vue@2和3，支持SSR，组件支持增量渲染即使大文件json也可以快速渲染。

官网：[vue-json-viewer/README_CN.md at master · chenfengjw163/vue-json-viewer (github.com)](https://github.com/chenfengjw163/vue-json-viewer/blob/master/README_CN.md)

## 安装

```
$ npm install vue-json-viewer@2 --save
// Vue2
$ npm install vue-json-viewer@3 --save
// Vue3
```



## 使用

第一步：在`src`的`plugins`目录下创建`jsonViewer.js`文件

第二步：写入以下内容

```js
import Vue from 'vue';
import jsonViewer from 'vue-json-viewer';
Vue.use(jsonViewer);

```

第三步：在入口文件`main.js`中引入

```js
// 引入vue-json-viewer
import './plugins/jsonViewer.js';
```

第四步：在组件中使用

```html
<json-viewer :value="jsonData"></json-viewer>

<json-viewer
  :value="jsonData"
  :expand-depth=5
  copyable
  boxed
  sort></json-viewer>
```

## 属性即事件请从官网上自行查找

