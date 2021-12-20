# npm包管理器

npm（全程Node package Manager，即node包管理器），是Node.js默认的、以JavaScript编写的软件包管理系统。

---

**npm就像是一个应用商店一样，想要什么就安装什么(前提是商店中有的)**

## 安装

下载并安装node.js可以直接安装上npm包管理器

## 下载配置

可以将下载网址配置成国内网站，这样下载速度较快。

使用淘宝镜像：`npm config set registry https://registry.npm.taobao.org`

## npm的使用指令

npm -v：查看版本，判断npm是否安装成功

npm install <Module Name@版本号>：使用npm命令安装模块（不加版本号直接安装最新版本）

npm install <Module Name@版本号> -g：全局安装 可以直接在命令行中使用（不加版本号直接安装最新版本）

npm update <Module Name>：更新到最新版本

npm list -g：查看所有全局安装模块

npm list <Module Name>：查看某个模块的版本号

npm uninstall <Module Name>：卸载模块

npm run <指令名>：运行指令

npm help ：可以查看很多指令

---

npm install -save <modul Name>：#-save在package文件的**dependencies**节点写入依赖。

npm install -save-dev <modul Name>：#-save-dev是在package文件的**devDependencies**节点写入依赖

**dependencies**：运行时的依赖，发布后，即生产环境下任然需要使用的模块

**devDependencies**：开发时的依赖，里面的模块是开发时使用的，发布时用不到，比如项目中使用的gulp，压缩css，js的模块。这些模块是我们项目部署后不需要的



## npm命令的一些简写

install：i

-save：-S

-dev：-D

global：g

## package.json配置文件

npm init ：声明配置文件

---



![package.json属性](../../前端图片/npm与yarn/package.json属性.png)

---

script：下边的命令可以执行，执行方式npm run 命令名

当在项目中时不会直接将node_modules这个大的文件夹传给你，只会将package.json配置文件发送给你。

接下来直接运行：npm install

该命令会按照package.json配置文件安装里面所有的module

## babel代码转换

![babel转换](../../前端图片/npm与yarn/babel转换.png)
