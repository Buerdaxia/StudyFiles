# `redis`入门介绍



##  简介

一个可以将数据存储在内存中，以键值对的方式存储的数据库。



## 5种常用数据类型介绍

`Redis`存储的是key-value结构的数据，其中key是字符串类型，value有5种常用的数据类型：

* 字符串 `string`
* 哈希 `hash`
* 列表 `list`
* 集合` set`
* 有序集合 `sorted set / zset`



**各自的特点：**

![01-Redis入门01](./assets/01-Redis入门01.png)





## `Redis`常用命令(Spring Data Redis)

### 字符串操作命令

Redis字符串类型常用命令：

| 语句                    | 作用                                            |
| ----------------------- | ----------------------------------------------- |
| SET key value           | 设置指定key的值                                 |
| GET key                 | 获取指定key的值                                 |
| SETEX key seconds value | 设置指定key的值，并将key的过期时间设为seconds秒 |
| SETNX key value         | 只有再key不存在时设置key的值(返回1成功，0失败)  |

>注意：
>
>语句各个单词中间是用空格隔开的



`Spring Data Redis`对应命令：

```java
/**
 * 操作字符串类型的数据
 */
@Test
public void testString() {
    // set get setex setnx

    // set
    redisTemplate.opsForValue().set("city", "北京");
    // get
    String city = (String)redisTemplate.opsForValue().get("city");
    System.out.println(city);

    // setex 可以设置过期时间
    redisTemplate.opsForValue().set("code", "1234", 1, TimeUnit.MINUTES);

    // setnx不存在的key才能设置
    redisTemplate.opsForValue().setIfAbsent("lock", "1");
    redisTemplate.opsForValue().setIfAbsent("lock", "2"); // 这句话应该设置不了
}

```







### 哈希操作命令

Redis hash 是一个string类型的field和value的映射表，**hash特别适合用于存储对象**，常用命令：

| 语句                 | 作用                                    |
| -------------------- | --------------------------------------- |
| HSET key field value | 将哈希表key中的字段field的值设置为value |
| HGET key field       | 获取存储在哈希表中指定字段的值          |
| HDEL key field       | 删除存储在哈希表中的指定字段            |
| HKEYS key            | 获取哈希表中所有字段(field)             |
| HVALS key            | 获取哈希表中所有的值(value)             |



`Spring Data Redis`对应命令：

```java
/**
 * 操作哈希类型数据
 */
@Test
public void testHash() {
    // HSET HGET HDEL HKEYS HVALS
    HashOperations hashOperations = redisTemplate.opsForHash();

    // HSET 设置key-value
    hashOperations.put("100", "name", "jack");
    hashOperations.put("100", "age", "18");

    // HGET 获取
    String name = (String) hashOperations.get("100", "name");
    System.out.println(name);

    // HKEYS
    Set keys = hashOperations.keys("100");
    System.out.println(keys);

    // HVALS
    List values = hashOperations.values("100");
    System.out.println(values);

    // HDEL 删除
    hashOperations.delete("100", "age");
}
```





### 列表操作命令

Redis 列表是简单的**字符串列表**，按照插入顺序排序，常用命令：

| 语句                      | 作用                                                    |
| ------------------------- | ------------------------------------------------------- |
| LPUSH key value1 [value2] | 将一个或多个值插入到**列表头部**                        |
| RPUSH key value1 [value2] | 将一个或多个值插入到**列表尾部**                        |
| LRANGE key start stop     | 获取列表指定范围内的元素(start -1 表示取出列表所有元素) |
| RPOP key                  | 移除并获取列表最后一个元素                              |
| LPOP key                  | 移除并获取列表第一个元素                                |
| LLEN key                  | 获取列表长度                                            |

>注意：
>
>语句开头的L和R是left和right的意思，从左侧查出，从右侧删除
>
>小技巧：
>
>取出列表所有元素：`LRANGE key start -1`



`Spring Data Redis`对应命令：

```java
/**
 * 操作列表类型的数据
 */
@Test
public void testList() {
    // lpush lrange rpop llen

    ListOperations listOperations = redisTemplate.opsForList();

    // lpush
    listOperations.leftPushAll("mylist1", "a", "b", "c");
    listOperations.leftPush("mylist1", "d");

    // lrange
    List mylist1 = listOperations.range("mylist1", 0, -1);
    System.out.println(mylist1);

    // rpop
    String popItem = (String) listOperations.rightPop("mylist1");
    System.out.println(popItem); //

    // llen
    Long size = listOperations.size("mylist1");
    System.out.println(size);

}
```







### 集合操作命令

Redis set是**string类型的无需集合**。集合成员是唯一的，集合中不能出现重复数据，常用命令：

| 语句                       | 作用                     |
| -------------------------- | ------------------------ |
| SADD key member1 [member2] | 向集合添加一个或多个成员 |
| SMEMBERS key               | 返回集合中的所有成员     |
| SCARD key                  | 获取集合的成员数         |
| SINTER key1 [key2]         | 返回给定所有集合的交集   |
| SUNION key1 [key2]         | 返回所有给定集合的并集   |
| SREM key member1 [member2] | 删除集合中一个或多个成员 |





`Spring Data Redis`对应命令：

```java
/**
 * 操作集合相关方法
 */
@Test
public void testSet() {
    SetOperations setOperations = redisTemplate.opsForSet();

    //sadd smembers scard sinter sunion srem

    // sadd
    setOperations.add("set1", "a", "b", "c", "d");
    setOperations.add("set2", "a", "b", "x", "y");

    // smembers
    Set set1 = setOperations.members("set1");
    System.out.println(set1);

    // scard
    Long size = setOperations.size("set1");
    System.out.println(size);

    // sinter
    Set intersect = setOperations.intersect("set1", "set2");
    System.out.println(intersect);

    // sunion
    Set union = setOperations.union("set1", "set2");
    System.out.println(union);

    // srem
    setOperations.remove("set1", "a", "b");
}
```









### 有序集合操作命令

Redis 有序集合也是**string类型元素的集合，且不允许有重复成员。每个元素都会关联一个double类型的分数**。常用命令：

| 语句                                        | 作用                                                   |
| ------------------------------------------- | ------------------------------------------------------ |
| ZADD key score1 member1 [score2 member2]... | 向有序集合添加一个或多个元素                           |
| ZRANGE key start stop [WITHSCORES]          | 通过索引区间返回有序集合(WITHSCORES：是否需要返回分数) |
| ZINCREBY key increment member               | 有序集合中对指定成员的分数加上增量increment            |
| ZREM key member [member...]                 | 移除有序集合中的一个或多个成员                         |





### 通用命令

Redis的通用命令是不分数据类型的，都可以使用的命令：

| 语句              | 作用                               |
| ----------------- | ---------------------------------- |
| KEYS pattern      | 查找所有符合给定模式(pattern)的key |
| EXISTS key        | 检查给定key是否存在                |
| TYPE key          | 返回key所存储的值的类型            |
| DEL key [key2...] | 该命令用于仔key存在时删除key       |

>都是操作key相关的(●'◡'●)

查询所有set的key：

```
keys set*
```

查询所有的key：

```
keys *
```





`Spring Data Redis`的操作命令：

```java
/**
 * 通用指令操作
 */
@Test
public void testCommon() {
    //keys exists type del

    Set keys = redisTemplate.keys("*");
  	// 查询所有开头为dish_的key
  	Set keys2 = redisTemplate.keys("dish_*");
    System.out.println(keys);

    // exists
    Boolean name = redisTemplate.hasKey("name");
    Boolean set1 = redisTemplate.hasKey("set1");

    // type
    for (Object key : keys) {
        DataType type = redisTemplate.type(key);
        System.out.println(type.name());
    }

    // del
    redisTemplate.delete("mylist1");
}
```







## 在Java中操作Redis

### Redis的Java客户端

Redis的Java客户端很多，常见的几种：

* Jedis
* Lettuce
* Spring Data Redis

`Spring Data Redis`是`Spring`的一部分，对`Redis`底层开发包进行了高度封装。在Spring项目中，建议使用这个





## Spring Data Redis使用方式

### 操作步骤：

![01-Redis入门02](./assets/01-Redis入门02.png)

一：导入

`pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



二：配置数据源

`application.yml`

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password: 1234
    database: 0 # redis默认启动就会创建16个数据库 0~15，不同数据库数据完全隔离
```

>注意：主要是要配置在spring下第一级



三：编写配置类，创建RedisTemplate对象

`config/RedisConfiguration.class`

```java
package com.sky.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.autoconfigure.condition.ConditionalOnSingleCandidate;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
@Slf4j
public class RedisConfiguration {

    @Bean // 加上bean注解，后面参数中的redisConnectionFactory就会被注入进来
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory){
        log.info("开始创建Redis模板对象...");

        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        // 设置Redis的连接工厂对象
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        // 设置Redis key的序列化器（string类型序列化器） 如果不设置key到时候可能会乱码可读性比较差
        redisTemplate.setKeySerializer(new StringRedisSerializer());

        return redisTemplate;
    }
}

```

>如果报红，可能是springboot版本过高，不会影响功能的



四：通过`RedisTemplate`对象来操作Redis

```java
// 5种类型的对象
ValueOperations valueOperations = redisTemplate.opsForValue();
SetOperations setOperations = redisTemplate.opsForSet();
HashOperations hashOperations = redisTemplate.opsForHash();
ListOperations listOperations = redisTemplate.opsForList();
ZSetOperations zSetOperations = redisTemplate.opsForZSet();
```

