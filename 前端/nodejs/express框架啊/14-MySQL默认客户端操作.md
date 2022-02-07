# MySQL默认客户端操作

`quit/exit;`

退出客户端

## 数据库的一些操作



`create database 数据库名 charset=utf8;`

创建指定数据库



`show databases;`

查看当前创建数据库



`select database();`

查看当前使用的数据库



`show cteate database 数据库名`

查看创建指定数据库的语句



`use 数据库名;`

使用指定数据库





`drop database 数据库名;`

删除指定数据库



## 数据表的一些操作

`show tables;`

展示当前使用数据库下所有表格

创建表

```
int unsigned 无符号整型
auto_increament 表示自动增长一般和主键在一起
not null 非空
primary key 主键
default 默认值
```

`create table 表名(字段 类型 约束[,字段 类型 约束]);`

```
create table test(name varchar(30) not null, age int unsigned);
```

**注意：创建表时，类型必须写，约束不是必须的**



删除表

`drop table 表名;`

示例：

```
drop table test;
// 删除test表
```





查看表的结构(如字段值呀，字段的类型呀等等)

`desc 数据表名;`

例如：

```
desc test;
```



### 修改表的结构

添加字段mascot(吉祥物)

`alter table 表名 add 列名 类型 约束...` 

示例：

```
alter table classes add mascot varchar(50);
```



修改字段，不重名版本（仅仅修改字段 类型 约束等 不修改字段名）

`alter table 表明 modify 列名 类型 约束...`

示例：

```
alter table classes modify mascot varchar(100);
// 将classes中的 mascot 字段 类型修改为 字符型大小为100
```



修改字段，同时修改字段名

`alter table 表明 change 旧列名 新列名 类型 约束...`

示例：

```
alter table classes change mascot jxw int unsigned;

//修改classes中的mascot字段改为jxw 并修改类型为 无符号整型
```



删除字段

`alter table 表名 drop 字段名`

示例：

```
alter table classes drop jxw;
//删除 classes表中的jxw字段
```



### 表的增删改查（curd）

增加（插入）

指定字段单一插入

`insert into 表名(字段1, 字段2...) values(值1, 值2...);`

主键字段可以用0 null default 来占位

示例：

```
insert into classes(id, name) values(1, '大前端');

//向表classes中id，name添加数据1和大前端
insert into classes(id, name) values(0, '大前端');
// 如果插入数据写0时，就是没写，id会自增（创建表格时添加了auto_increment）
```



全部插入，向表中所有字段插入数据

`insert into 表名 values(值1, 值2, 值3...)`

有多少字段插入多少值

学生表结构

```
+--------+-------------------------------+------+-----+---------+----------------+
| Field  | Type                          | Null | Key | Default | Extra          |
+--------+-------------------------------+------+-----+---------+----------------+
| id     | int(10) unsigned              | NO   | PRI | NULL    | auto_increment |
| name   | varchar(30)                   | NO   |     | NULL    |                |
| age    | int(10) unsigned              | YES  |     | NULL    |                |
| high   | decimal(3,2)                  | YES  |     | NULL    |                |
| gender | enum('男','女','保密','中性') | YES  |     | 保密    |                |
| cls_id | int(10) unsigned              | YES  |     | NULL    |                |
+--------+-------------------------------+------+-----+---------+----------------+
```

示例：

```
insert into students values(1, '钱不二', 22, 1.86, '男', 1);
// 向学生表中插入数据

insert into students(name) values('李四');
// 部分插入，只规定name值为李四，其他全部默认值

```



多行插入

`insert into 表名(列名) values(值1), (值2), (值3);`







