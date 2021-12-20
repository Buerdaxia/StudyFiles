# vue项目格式化的问题

## 问题一：**vscode中的格式化文档是不符合eslint检查的。**

解决方案：

第一步：在文件中创建.prettercc文件

第二部：添加

```json
{
    "semi": false,
    "singleQuote": true
}
```

第一个标识不加封号，第二个标识用单引号标识字符串

两步即可解决。

## 问题二：如果遇到space-before-function-paren问题

在.eslintrc.js中找到rules

在下面添加以下代码

```js
"space-before-function-paren": 0
```

如果想打封号；添加

```js
"semi": [0]
```



