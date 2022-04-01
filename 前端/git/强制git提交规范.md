# 强制git提交规范

让程序员保持规范，是一种奢望————沃布芝到是谁硕德



## 安装

>首先安装`commitlint`
>
>```
>npm install --save-dev @commitlint/config-conventional@12.1.4 @commitlint/cli@12.1.4
>```

>其次安装`husky`
>
>```
>npm install husky@7.0.1 --save-dev
>npx husky install
>```

之后会发现多了一个`.husky`的文件夹



## 添加

一、在`package.json`中添加新增指令

```js
"prepare":"husky install"
```

并且执行：`npm run prepare`



二、添加配置文件

>1.运行 
>
>```
>npx husky add .husky/commit-msg
>```
>
>2.之后会多出：`commit-msg`文件在`.husky`文件夹下
>
>3.将husky和commit-lint进行关联在`commit-msg`文件夹下添加(替换undefined)
>
>```js
>npx --no-install commitlint --edit
>```
>
>



