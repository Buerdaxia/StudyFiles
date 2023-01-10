# git同时配置多个SSH

目的：一台电脑可以让github、gitee、gitlab等账号同时存在，不同账号配置不同的密钥。



## 创建config文件

地址：`C:\Users\用户名\.ssh`

文件名：`config`

>注意：这里的文件格式是没有文件后缀名儿的，可以复制一份rsa文件，然后进行修改，或者直接创建一个出来把后缀名删掉

文件内容：

```
# gitee
Host gitee.com # 代表gitee的git代码仓库地址
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.gitee

# github
Host github.com # 代表github的git代码仓库地址
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.github

# gitlab
Host xx.xx.xx.xx:8888 # 代表公司gitlab的git代码仓库地址
HostName xx.xx.xx.xx:8888
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.gitlab

```



## 创建对应仓库的ssh

手动创建`id_rsa.gitlab`、`id_rsa.gitee`、`id_rsa.github`密钥文件。

执行命令：

```
ssh-keygen -t rsa -C "对应仓库邮箱地址@qq.com" -f ~/.ssh/id_rsa.gitee
```

每个仓库都要执行一遍，和上面`config`文件中的仓库一一对应



创建完毕后：在`C:\Users\用户名\.ssh`地址下会多6个文件，一个公钥一个私钥，22对应的



## 配置公钥

将创建好的公钥（复制带.pub文件的内容到各个仓库中）配置到每个仓库中（gitee、github、gitlab）





## 验证key是否正常工作

输入命令：

```
ssh -T git@gitee.com
# 使用 github的话，则改为
ssh -T git@github.com
```



如果出现：`Are you sure you want to continue connecting (yes/no)?`

输入：`yes`

如果出现successfully authenticated表示授权成功