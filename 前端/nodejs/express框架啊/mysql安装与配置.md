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

sql_mode=NO_
```

