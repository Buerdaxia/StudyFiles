# nodemon

nodemon是一种工具，可以自动检测到目录中的文件更改时通过重新启动应用程序来调试基于node.js的应用程序。



## 安装

```
npm install -g nodemon
// 或
npm install --save-dev nodemon
```



## 使用

```
nodemon 文件名
```

```
nodemon 文件名 localhost 6677 //在本地6677端口上启动node服务
```



## 如果使用vscode中的code-runner

在设置中的settings.json中添加

```json
// code-runner的扩展，修改执行
  "code-runner.executorMap": {
    "javascript": "nodemon",
  },
```

修改后直接右键，然后Run Code即可

