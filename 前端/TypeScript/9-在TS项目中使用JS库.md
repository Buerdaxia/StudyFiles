# 在TS项目中使用`JS`库

前提：**有一些小型的`JS`库可能没有类型定义文件需要我们自己安装，但是一些主流的新版本库都会自带类型定义文件**

在`ts`项目中引入一些`JS`的库，会发现我们引入之后，说引入的库未定义。

原因是，`TS`都是通过`type`来对代码进行鉴别，而`JS`库肯定是没有类型(`type`)的，我们可以在中间安装一个**类型定义文件**，用来帮助`TS`识别`JS`库



使用方法：

```
npm install @types/{library name}
```

在安装的`JS`库前面加上`@types/`

例如：

```js
// 我们现在安装一个faker.js库
npm install -g faker

// 为了能在TS项目中使用 还要安装一个
npm install @types/faker
```



