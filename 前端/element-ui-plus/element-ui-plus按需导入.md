# element-ui-plus按需导入

安装`element-plus`:

```
npm install element-plus --save
```



安装两个自动引入插件：

```
npm install -D unplugin-vue-components unplugin-auto-import
```







前面步骤不再讲解，官网有：关键在配置`Webpack`配置文件

vue脚手架配置`webpack`的配置文件名称为`vue.config.js`





# 自动导入配置：





## webpak打包配置

官网要求在`webpack`配置文件中写入：

```js
// webpack.config.js
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = {
  // ...
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
}
```



## vue脚手架打包配置

创建`vue.config.js`并写入：

```js
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = {
  // ...
  configureWebpack: config => {
    config.plugins.push(
      AutoImport({
        resolvers: [ElementPlusResolver()]
      })
    ),
      config.plugins.push(
        Components({
          resolvers: [ElementPlusResolver()]
        })
      )
  }
}

```



## vite的配置（推荐使用）

在`vite.config.js`:

```js
import {fileURLToPath, URL} from 'node:url';

import {defineConfig} from 'vite';
import vue from '@vitejs/plugin-vue';

// elment-plus
import AutoImport from 'unplugin-auto-import/vite';
import Components from 'unplugin-vue-components/vite';
import {ElementPlusResolver} from 'unplugin-vue-components/resolvers';
//


// https://vitejs.dev/config/
export default defineConfig({
	plugins: [
    // elment-plus
		AutoImport({
			resolvers: [ElementPlusResolver()]
		}),
		Components({
			resolvers: [ElementPlusResolver()]
		}),
    //
		vue()
	],
	resolve: {
		alias: {
			'@': fileURLToPath(new URL('./src', import.meta.url))
		}
	}
});

```







# 手动按需导入

创建`plugins.js`文件引入想要的组件：

```js
// elment-plus.js
import { createApp } from 'vue'
import { ElButton } from 'element-plus'


const components =  [ElButton];
export default  {
	components
}
```



在`main.js`中，循环注册：

```js
import './assets/main.css';
import {createApp} from 'vue';
import {createPinia} from 'pinia';
import 'element-plus/dist/index.css'
import elComp from '@/plugins/elment-plus.js';

import App from './App.vue';
import router from './router';

const app = createApp(App);

app.use(createPinia());
app.use(router);

// 循环注册
elComp.forEach(item => {
  app.use(item)
})


app.mount('#app');

```

