# mysql安装与配置

## 解压

如果拿到的是压缩包，将软件解压到自定义目录下：

例如：E:\MYSQL\mysql-5.6.41-winx64（**路径下禁止出现中文**）



配置文件`my-default.ini`文件名修改为:`my.ini`（建议先备份一份再修改）



复制以下内容到`my.ini`：

```
# 设置mysql客户端默认字符集
defalut-character-set=utf8
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=E:\MYSQL\mysql-5.6.41-winx64
# 设置mysql数据库的数据的存放目录
basedir=E:\MYSQL\mysql-5.6.41-winx64\data
# 允许最大连接数
max_connections=200
# 服务端使用默认字符集utf8
character-set-server=utf8
# 创建新表时使用的默认存储引擎
default-storage-engine-INNODB

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

## 安装

以**以管理员身份启动**cmd黑窗口，进入到mysql文件夹下的bin目录下，执行：

```
mysql --install mysql --defaults-file="E:\MYSQL\mysql-5.6.41-winx64\my.ini"
```

提示：`Service successfully installed`说明安装成功。



## 使用

首先一定先要启动MySQL服务，再打开客户端

```
启动：net start mysql
```



默认启动方式：

第一步：进到`mysql`安装目录下的`bin`目录下

第二步：执行（打开客户端）：

```
mysql -u root -p

// -u 是用户名 后面跟着用户名
// -p 是密码 后面跟着密码
```

第三步：输入密码

输入第二步命令后：会提示`Enter pasasword:`

然后输入设置的密码即可

**启动、停止、卸载mysql服务指令：**

1. 启动：net start mysql
2. 关闭：net stop mysql
3. 卸载：mysqld -remove



## 配置环境变量

如果想要全局使用指令，那么需要配置一个系统变量

win10系统操作顺序

第一步：打开环境变量

双击我的电脑(计算机)-->右键：属性-->点击高级系统设置-->点击环境变量



第二步：新建系统变量

点击新建：

变量名：MYSQL_HOME

变量值：mysql的安装路径

例如:`E:\MYSQL\mysql-5.6.41-winx64`



第三步：添加`path`

点击系统变量中的`Path`添加如下语句

```
%MYSQL_HOME%\bin
```

意思：指去这个路径下去找命令
