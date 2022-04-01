# element-ui-plus按需导入

前面步骤不再讲解，官网有：关键在配置`Webpack`配置文件

vue脚手架配置`webpack`的配置文件名称为`vue.config.js`

自动导入配置：

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

