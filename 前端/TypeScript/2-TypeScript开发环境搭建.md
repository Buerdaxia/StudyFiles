# TypeScript开发环境搭建

1. 安装`Node.js`
2. 使用`npm`全局安装`typescript`，`ts-node`

```
npm install typescript -g ts-node
```

3. 创建一个`ts`文件
4. 使用`tsc`对`ts`文件进行编译
   1. 进入命令行
   2. 进入`ts`文件所在目录
   3. 执行命令 `tsc xxx.ts`（就会产生一个`xxx.js`代码）



ts-node:（就是我们执行ts文件时，第一步`tsc xxx.ts` 第二步，用node执行`xxx.js`），这个小模块可以让我们2步合成一步，直接`ts-node xxx.ts`





## 小满版本的

>第一步：

```
npm install @types/node -D
```

>第二步：

```
 npm install ts-node -g
```



> 第三步直接执行ts文件，可以看到输出结果

```
ts-node app.ts
```



