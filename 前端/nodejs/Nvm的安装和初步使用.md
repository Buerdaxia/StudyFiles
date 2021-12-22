# Nvm的安装和初步使用

nvm可以用来更好管理不同版本的本地node存储在我们的电脑中。

nvm（node.js version manager 的简写）翻译过来nodsjs版本管理器。

## 安装

下载：[Releases · coreybutler/nvm-windows · GitHub](https://github.com/coreybutler/nvm-windows/releases)



注意！！！：**如果电脑有node.js首先卸责node.js再进行安装**

nvm的路径：C:\Users\10854\AppData\Roaming\nvm

node.js的路径：C:\Program Files\nodejs



监测nvm是否安装成功

重新打开cmd：nvm -v



配置nvm：复制下面两行代码到（C:\Users\xxx\AppData\Roaming\nvm）路径下的settings.txt中

加快下载罢了(●ˇ∀ˇ●)

```
node_mirror:https://npm.taobao.org/mirrors/node/
npm_mirror:https//npm.taobao.org/mirrors/npm/
```



## 操作

nvm ls-remote：查看所有node版本

nvm version：查看当前nvm版本

**nvm list：查看当前安装那些版本的node.js（常用）**

**nvm install 版本号：安装指定版本的node.js（常用）**

**nvm uninstall 版本号：卸载指定版本的node.js（常用）***

**nvm use 版本号：选择指定版本的node.js（常用）**

```
# 安装指定版本
nvm install 10.15.0

# 安装最新版本
nvm install latest

# 使用该版本10.15.0
nvm use 10.15.0

# 清屏
cls
```

**注意：**如果使用那条命令失败了，记得以管理员身份运行。
