# prettier的使用

## 为项目配置prettier

使用：visual studio code

第一步：

安装prettier插件

第二步：

右键->选择格式化文档，方法是使用...->配置默认格式化程序->Prettier



第三步：

项目中创建`.prettierrc`文件，该文件为当前项目的`prettier`配置文件

>不同项目的配置文件可能不同

```js
{
	"semi": false,
	"singleQuote": true,
	"trailingComma": "none"
}
```