# vue-cli4脚手架配置多环境



## 一：创建文件

在 vue-cli4 的脚手架构建的项目根目录中新建一个 .env.[model] 文件，model 为一个变量，可以通过这个 model 的变量值来判断当前属于哪个环境。model 可以根据你的需求来修改, 该文件中写上对应的键值对。你需要多少个环境，就创建多少个 .env.[model] 在根目录中，文件如下图。

```
# 预发环境
.env.beta
 
# 开发环境
.env.development
 
# 生产环境
.env.production
 
# 测试环境
.env.test
```

