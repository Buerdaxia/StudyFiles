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

```mysql
drop table test;
-- 删除test表
```





查看表的结构(如字段值呀，字段的类型呀等等)

`desc 数据表名;`

例如：

```mysql
desc test;
```



### 修改表的结构

**添加字段**

`alter table 表名 add 列名 类型 约束...` 

示例：

```mysql
alter table classes add mascot varchar(50);
-- 添加mascot(吉祥物)字段
```



修改字段，不重名版本（仅仅修改字段 类型 约束等 不修改字段名）

`alter table 表明 modify 列名 类型 约束...`

示例：

```mysql
alter table classes modify mascot varchar(100);
-- 将classes中的 mascot 字段 类型修改为 字符型大小为100
```



修改字段，同时修改字段名

`alter table 表明 change 旧列名 新列名 类型 约束...`

示例：

```mysql
alter table classes change mascot jxw int unsigned;

--修改classes中的mascot字段改为jxw 并修改类型为 无符号整型
```



删除字段

`alter table 表名 drop 字段名`

示例：

```mysql
alter table classes drop jxw;
--删除 classes表中的jxw字段
```



### 表的增删改查（curd）

#### 增加（插入）

**指定字段单一插入**

`insert into 表名(字段1, 字段2...) values(值1, 值2...);`

主键字段可以用0 null default 来占位

示例：

```mysql
insert into classes(id, name) values(1, '大前端');

--向表classes中id，name添加数据1和大前端
insert into classes(id, name) values(0, '大前端');
-- 如果插入数据写0时，就是没写，id会自增（创建表格时添加了auto_increment）
```



**全部插入，向表中所有字段插入数据**

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

```mysql
insert into students values(1, '钱不二', 22, 1.86, '男', 1);
-- 向学生表中插入数据

insert into students(name) values('李四');
-- 部分插入，只规定name值为李四，其他全部默认值

```



**多行插入**

`insert into 表名(列名) values(值1), (值2), (值3);`



#### 修改

`update 表名 set  列1=值1, 列2=值2...`



**全部修改**

`update 表名 set 列1 = 值`

示例：

```mysql
update students set age = 16;
--  修改students内的所有age = 16
```



**按条件修改**

`update 表名 set 列1=值1... where 条件 `

示例：

```mysql
update students set age = 20 where id=3;
-- 修改students表下，当id=3 将age修改为20
```



**根据条件修改多个值**

`update 表名 set 列1=值1, 列2=值2... where 条件;`

示例：

```mysql
update students set age=15, high=1.80 where id=2;
-- 修改students表下的，id=2的 age修改为15 high修改为1.80
```



#### 查询的基本使用

方式一：

`select * 或 列1, 列2... from 表名 where 条件;`

方式二:

`select 表名.字段, 表名.字段... from 表名`

注意：查询出来字段的顺序，和书写的顺序有关

**查询所有列**

`select * from 表名;`

示例:

```mysql
select * from students;
-- 查询students表下所有列
```



**根据条件查询**

`select * from 表名 where 条件;`

示例:

```mysql
select * from students where id = 1;

-- 查询students表下id等于1的所有列
```



**查询指定列**

`select 列1, 列2... from 表名 where 条件;`

示例:

```mysql
select name, age from students;

select name, age from students where id = 1;
```



**可以使用as给列或者表指定别名**

`select 字段[as 别名], 字段[as 别名] from 数据表;`

示例:(给字段起别名)

```mysql
select name as '姓名', age as '年龄' from students;

-- 查询students表下的name,和age 但是展示是以'姓名'和'年龄'
```



示例：(给表起别名)

```mysql
select s.name, s.age from students as s;
-- 给students表名起别名为s，前面的书写就可以简写了
```



**给查询结果消除重复项**

`distinct 字段;`

示例：

```mysql
select distinct gender from students;
-- 查询students下的gender字段，并且合并重复项
```

也可以用分组的方式来删除重复项

```mysql
select gender from students group by gender;
-- 查询students下的gender字段，并且合并重复项
```



#### 删除

删除分为两种

1. 物理删除
2. 逻辑删除

物理删除：

**真正的从数据库中将这数据删除**，再也查询不到了例如`delete from 表名 where 条件;`

示例：

```mysql
delete from students where id = 4;
-- 物理删除，删除id等于4的那一条记录
```









逻辑删除：（软删除）

并不会真正的删除这条数据，而是一般会再表格最后一列添加一个新列（如果有就不用添加了），列名为`is_delete`

如果这行数据要删除，那么就将is_delete这列对应的位置插入1或这true，标识这条数据被删除了，**但是数据还是再数据库中，只是不再使用了。**相当于将这条数据丢到回收站去，有需要时可以恢复回来



示例：

添加一个字段用来标识该条信息是否继续使用。

```mysql
alter table students add is_delete bit default 0;
-- 添加一个 is_delete字段，类型为bit 默认值为0
```

修改`is_delete`字段，1为删除，

```mysql
update students set is_delete = 1 where id = 1;
-- 删除id=1的记录(软删除)
```



### 高级查询

#### 条件查询

查询语句的条件中可以写**比较运算符**（>,<,>=,<=, =, !=d等价于<>）

`select ... from 表名 where 条件`;

示例：

```mysql
select * from students where age > 18;
-- 查询age大于18的所有数据
```



查询语句条件中也可以写**逻辑运算符**

逻辑运算符：`and / between ... and ...`

示例:

```mysql
select * from studnets where age > 18 and age < 28;
-- 查询年龄大于18小于28的数据（不包含18和28）

select * from studnets where age >= 18 and age =< 28;
-- 查询年龄大于18小于28的数据（包含18和28）

select * from studnets where age between 18 and 28;
-- 查询年龄大于18小于28的数据（包含18和28）
```



示例2：

```mysql
select * from students where age > 18 and gender = '女';
-- 查询18岁以上女生的信息
select * from students where age > 18 and gender = 2;
// 枚举类型可以用数值来标识
gender enum('男','女','中性','保密') default '保密',
-- 1就是男，2就是女，3是中性, 4是保密
```





逻辑运算符：`or`

示例：

```mysql
select * from students where age > 18 or height >= 180.00;
-- 18岁以上或者身高大于等于180.00的数据
```



逻辑运算符:`not`

示例：

```mysql
select * from students where not(age > 18 and gender = 2);
-- 不在 18岁以上的女性 这个范围内的信息
```



#### 模糊查询

`where name like xxx`

1. % 替换任意个
2. _ 替换一个

示例1：

```mysql
select * from students where name like '小%';
-- 查询students表中姓名中以“小”开始的名字的学生信息
```



示例2：

```mysql
select * from students where name like '%小%';
-- 查询students表中姓名中含有“小”字的名字的学生信息
```



示例3：

```mysql
select * from students where name like '__';// 这里有两个_
-- _表示一个
-- 查询姓名为两个字的学生信息
```



示例4：

```mysql
select * from students where name like '__%';
-- 查询姓名至少是2个字的学生信息
```



#### 范围查询

`in(1, 3, 8)`表示实在一个非连续的范围内。**其中的 1，3， 8是确切的值不是范围**

示例：

```mysql
select * from students where age in (18, 34);
等价于
select * from students where age = 18 or age = 34;
-- 查询年龄为18或者34的学生信息
```



`not in`和上面相反，表示不在一个非连续的范围内

示例：

```mysql
select * from students where age not in (18, 34);
-- 查询年龄不是18或者34的学生信息
```



`between ... and ...`表示在一个连续的范围内

示例：

```mysql
select * from students where age between 18 and 34;
-- 查询年龄在18到34范围内的学生信息(包含18和 34)
```



`not between ... and ...`表示在不在一个连续的范围内

示例：

```mysql
select * from students where age not between 18 and 34;
-- 查询年龄不在在18到34范围内的学生信息
```



#### 空判断

`is null`判断为空

示例：

```mysql
select * from students where height is null;
-- 查询学生表这种身高为空的学生信息
```



`is not null `判断非空

示例：

```mysql
select * from students where height is not null;
-- 查询学生表这种身高不为空的学生信息
```





#### 排序查询

`order by 字段 xxx`  xxx中填排序规则，默认是从小到大升序

1. asc 从小到大排序，即升序
2. desc 从大到小排序，即降序

示例：

```mysql
select * from students where gender='男' and age between 18 and 34 order by age;
-- 查询年龄在18到34之间男性的学生信息，并且按照年龄从小到大排序
```

示例2:

```mysql
select * from students where gender=2 and age between 18 and 34 order by height desc;
-- 查询年龄在18到34之间的女性学生信息，并且按照身高由高到低排序
-- 注意因为gender是枚举类型所以可以用2 正常应该写为gender='女'
```



`order by 字段1 xxx, 字段2 xxx...`

排序第一条件是字段1,第二条件是字段2....以此类推

示例3：

```mysql
select * from students where gender=2 and age between 18 and 34 order by 
height desc, age asc;
-- 查询年龄在18到34之间的女性学生信息，并且按照身高由高到低排序,如果身高相同，那么就按照年龄从小到大排序
```



示例4：

```mysql
select * from students where gender=2 and age between 18 and 34 order by 
height desc, age asc, id desc;
-- 如果身高相同按照年龄从小到大排序，如果年龄也一样了，按照id从大到小排序
```



#### 聚合函数

注意：**聚合函数不能将null计算在内**，就是不会计算null

注意2：**聚合函数作为条件时，只能和having配合使用，不能和where使用**

`count`：总数

注意：count不会删除重复项，会计算所有的

示例：

```mysql
select count(*) from students where gender = '男';
-- 查询男生有多少人
```



` max`：最大值

示例：

```mysql
select max(age) from students;
-- 查询年龄最大的学生的年龄
```

示例2：

```mysql
select max(height) from students where gender='女';
-- 查询身高最高的女生的身高
```



`min`：最小值

示例：

```mysql
select min(height) from students where gender='女';
-- 查询身矮最高的女生的身高
```



`sum`：求和

示例：

```mysql
select sum(age) from students;
-- 计算所有学生年龄的总和
```



`avg`：平均值

示例：

```mysql
select avg(age) from students;
等同于
select sum(age)/count(*) from students;
-- 计算所有学生年龄的平均值
```



`round(数值, 保留位数)`：小数保留

```mysql
select round(avg(age), 2) from students;
-- 计算所有学生年龄的平均值并且将平均值保留2位
```



#### 分组查询

`select 分组的字段 from 表名 group by 分组字段`

关键字：每种，每类，这类字眼就是分组

技巧：当以什么字段分组时，最好将该字段展示出来

示例：

```mysql
select gender from students group by gender;
-- 按照性别分组，查询所有的性别
```



示例2:

```mysql
select gender, count(*) from students group by gender;
-- 计算每种性别的人数
```



`group_concat(...)`：可以合并每一组的某一字段

示例3：

```mysql
select gender, group_concat(name) from students group by gender;
-- 查询同种性别中的姓名
```



示例4：

```mysql
select gender, avg(age) from students group by gender;
-- 查询每组性别的平均年龄
```



**分组条件的设置**

`having 条件`：相当与每个分组的where，后面跟条件

注意：**聚合函数作为条件时，只能和having配合使用，不能和where使用**

示例：

```mysql
select gender, avg(age), group_concat(name) from students group by gender having avg(age) > 30;
-- 查询平均年龄超过30岁的性别，以及姓名
```



示例2：

```mysql
select gender, count(*) from students group by gender having count(*) > 2;
-- 查询每种性别中的人数多于2个的信息
```



#### 分页查询（套公式就完事儿了）

`limit start, count`

注意：**`limit`一般放在最后**

`start`:是跳过多少条记录

`count`:是每页显示个数

仔细想想是不是这个道理，再看看公式(●'◡'●)

**公式**：`limit (要显示第几页-1) * 每页分多少个, 每页业分多少个`（**很重要**）

示例:

```mysql
select * from students limit 5;
-- 查询前5个数据
```





示例2：

```mysql
select * from students limit 2;// 缩写版
-- 每页分2个，要显示第1页

select * from students limit 0, 2;// 通用公式版本
```

示例3：

```mysql
select * from students limit 2, 2;
-- 每页分2个，显示第2页
```



示例4：

```mysql
select * from students limit 4, 2;
-- 每页分2个，显示第3页
```



示例5：

```mysql
select * from students order by age asc limit 10, 2;
-- 每页分2个，显示第6页的信息，按照年龄从小到大排序
-- 注意要先整体排序，再进行分页
```



#### 连接查询（多表之间的查询）

`inner join ... on`

`select ... from 表1 inner join 表2 on 连接条件 `

连接条件一般是用来区分是否为有效数据。

示例:

```mysql
select * from students inner join classes on students.cls_id = classes.id;
-- 查询有能够对应班级的学生以及班级信息
```

示例2：

```mysql
select students.name, classes.name from students inner join classes on students.cls_id = classes.id;
-- 查询班级和姓名
-- 简写 可以给表用as起别名儿
select s.name, c.name from students as s inner join classes as c on s.cls_id = c.id;
```



示例3：

```mysql
select s.*, c.name from students as s inner join classes as c on s.cls_id = c.id;
-- 查询 又能够对应班级的学生以及班级信息，显示学生的所有信息students.*,显示班级名称classes.id
```

#### 子查询

**一个查询的结果，作为另外一个查询的一部分**

标量子查询：子查询返回的结果是一个数据（一行一列）

列子查询：返回结果是一列（一列多行）

行子查询：返回结果是一行（一行多列）



示例：

```mysql
select * from students where height > (select avg(height) from students);
-- 查询出高于平均身高的信息(height)
```

