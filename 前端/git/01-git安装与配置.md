# git安装与配置

## 安装

安装没啥讲的，去官网找到对应的版本，然后下载下来，之后一路next+install即可。







## 小配置

下载完毕后，需要对git进行一些小配置，例如配置以下`username`，`email`之后可以通过这两条信息判断出提交代码的人是谁。

配置指令：

```js
git config --global --list  // 检查当前配置的name和email


git config --global user.name '钱恩慈1' // 设置当前username为钱恩慈1

git config --global user.email '1085445531@qq.com'  // 设置邮箱为10.. 一般都会设置为公司配置给你的邮箱
```

