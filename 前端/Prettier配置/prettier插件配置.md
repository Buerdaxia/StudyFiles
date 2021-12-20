# prettier插件配置

{

  // 主题

  "workbench.colorTheme": "SynthWave '84",

  // vscode默认启用了根据文件类型自动设置tabsize的选项

  "editor.detectIndentation": false,

  // 重新设定tabsize

  "editor.tabSize": 2,

  // #值设置为true时，每次保存的时候自动格式化；值设置为false时，代码格式化请按       shift+alt+F

  "editor.formatOnSave": true,

  // 开启 vscode 文件路径导航

  "breadcrumbs.enabled": true,

  // 换行属性

  "prettier.printWidth": 100,

  // 箭头函数参数括号 avoid 能不加时就不加

  "prettier.arrowParens": "avoid",

  // #prettier去掉代码结尾的分号 

  "prettier.semi": false,

  // 无尾逗号

  "prettier.trailingComma": "none",

  // #使用带引号替代双引号 

  "prettier.singleQuote": true,

  // #让函数(名)和后面的括号之间加个空格

  "javascript.format.insertSpaceBeforeFunctionParenthesis": false,

  // #这个按用户自身习惯选择 

  "vetur.format.defaultFormatter.html": "js-beautify-html",

  // #让vue中的js按编辑器自带的ts格式进行格式化 

  "vetur.format.defaultFormatter.js": "vscode-typescript",

  "vetur.format.defaultFormatterOptions": {

      "js-beautify-html": {
    
      // 多行属性
    
      "wrap_attributes": "auto"
      /*
      	auto：属性不换行，排成一行
      	force:从第二个属性强制换行，默认缩进
      	force-aligned: 以排成列的形式强制换行
      	force-expand-multiline:默认值，每一个属性都强制换行
      */
    },

  },

  "prettier.useTabs": true,

  "[javascript]": { 

    "editor.defaultFormatter": "esbenp.prettier-vscode"

  },

  "[json]": {

    "editor.defaultFormatter": "esbenp.prettier-vscode"

  },

  "[vue]": {

    "editor.defaultFormatter": "octref.vetur"

  },

  "[html]": {

    "editor.defaultFormatter": "vscode.html-language-features"

  },

  "window.zoomLevel": 1

}



//beautify插件配置

// {

//   "workbench.colorTheme": "SynthWave '84",

//   "editor.formatOnSave": true,

//   "editor.fontSize": 14,

//   "editor.tabSize": 2,

//   "editor.detectIndentation": false,

//   "update.mode": "none",

//   "search.followSymlinks": false,

//   "[javascript]": {

//     "editor.defaultFormatter": "HookyQR.beautify"

//   },

//   "[html]": {

//     "editor.defaultFormatter": "HookyQR.beautify"

//   },

//   "javascript.updateImportsOnFileMove.enabled": "always",

//   "beautify.language": {

//     "html": [

//       "htm",

//       "html",

//       "vue"

//     ],

//     "js": {

//       "type": [

//         "javascript",

//         "json"

//       ],

//       "filename": [

//         ".jshintrc",

//         ".jsbeautify"

//       ]

//     },

//     "css": [

//       "css",

//       "scss",

//       "less"

//     ]

//   },

//   // 全局自定义配置详细看官网文档

//   "beautify.config": {

//     "brace_style": "none,preserve-inline",

//     "space_after_named_function": false, //函数名后空格

//     "indent_size": 2,

//     "indent_char": " ",

//     "wrap_attributes": "auto", //换行属性

//     "jslint_happy": false,

//     "unformatted": [],

//     "css": {

//       "indent_size": 2

//     }

//   },

//   "[vue]": {

//     "editor.defaultFormatter": "HookyQR.beautify"

//   },

//   "window.zoomLevel": 1

// } 

## vue中在根目录下创建.prettierrc

```
{
    "semi": false,
    "singleQuote": true
}
```

