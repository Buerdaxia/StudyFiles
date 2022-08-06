# vite项目配置vscode

>第一步：项目根目录下创建jsconfig.json



>第二步：写入

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "allowSyntheticDefaultImports": true,
    "module": "ES2015"
  }
}
```

解释：

`"baseUrl": "."`：指定项目源代码的起始路径.就是当前文件夹

`"allowSyntheticDefaultImports": true`：是如果一个模块没有默认导出的话任然允许import...from...

`"module": "ES2015"`：指使用es6模块导入导出方式

