# Spring Cache



## 介绍

Spring Cache是一个框架，实现了基于**注解**的缓存功能，只需要简单地添加一个注解，即可实现缓存功能。



Spring Cache 提供了一层抽象，底层可以切换不同地缓存实现，例如：

* EHCache
* Caffeine
* **Redis**

>注意：
>
>我们在springboot项目中，如果使用的是redis客户端，也就是`Spring Data Redis`，spring cache的底层实现就会使用redis



坐标信息：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-cache</artifactId>
  <version>2.7.3</version>
</dependency>
```





## 常用注解(重要)



| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @EnableCaching | 开启缓存注解功能，通常加在启动类上                           |
| @Cacheable     | 在方法执行前闲检查缓存中是否有数据，如果有数据，则会直接返回缓存数据，如果没有缓存数据，调用方法将方法返回值放入缓存中(**又取又放**) |
| @CachePut      | 将方法的返回值放入缓存中（**只放**）                         |
| @CacheEvict    | 将一条或多条数据从缓存中**删除**                             |





### `@EnableCaching`

基本用法：

```java
@Slf4j
@SpringBootApplication
@EnableCaching // 在启动类上添加即可
public class CacheDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(CacheDemoApplication.class,args);
        log.info("项目启动成功...");
    }
}
```





### `@CachePut`

>使用的比较少

基本用法：

```java
@PostMapping
// redis中的key的格式 cacheNames::key
@CachePut(cacheNames = "userCache", key = "#user.id") // key这样写，这里是拿到user的id 保证和形参user名称一致
// @CachePut(cacheNames = "userCache", key = "#result.id") // 对象导航，获取的是返回值的user
// @CachePut(cacheNames = "userCache", key = "#p0.id"); // 获取第一个形参也就是还是user
public User save(@RequestBody User user){
    userMapper.insert(user);
    return user;
}
```

>注意点1：
>
>为什么第一种写法拿到的是形参的user，却也能得到存储进数据库的id呢？
>
>第一：是应为我们在mapper或xml中设置了id返回，返回了存储进msql后的id，并赋值给了user.id
>
>第二：形参user和最后返回值的user是同一个引用，他们都指向一个user所以是一致的，mybatis将id赋值的user就是形参的那个user

>注意点2：
>
>一定要注意cacheNames，相关业务的名字要保持一致，例如我们现在是user相关的取名为userCache，那么后续的操作也需要将cacheNames取名为userCache（和user业务相关的）

>注意点3：
>
>这里框架帮我们存储的值，即使当前方法调用结束后的返回值



调用`save`方法后：

![01-springCache入门介绍01](assets/01-springCache入门介绍01.png)







###　`@Cacheable`

基本用法：

```java
@GetMapping
// 缓存逻辑：先查缓存，如果缓存有直接返回，如果没有查询数据库返回，并将返回值放入缓存中
@Cacheable(cacheNames = "userCache", key = "#id")
public User getById(Long id){
    User user = userMapper.getById(id);
    return user;
}
```

>注意点：
>
>该注解会在方法执行前就去操作，本质使用代理对象

>注意点2：
>
>这里框架帮我们存储的值，即使当前方法调用结束后的返回值



使用地点：

* 一般是获取相关接口使用



### `@CacheEvict`

基本用法：

```java
@DeleteMapping
// 删除一条缓存记录
@CacheEvict(cacheNames = "userCache", key = "#id") // 动态删除userCache::id
public void deleteById(Long id){
    userMapper.deleteById(id);
}

@DeleteMapping("/delAll")
// 删除userCache下面所有缓存记录
@CacheEvict(cacheNames = "userCache", allEntries = true)
public void deleteAll(){
    userMapper.deleteAll();
}
```

>注意：
>
>删除指定一条记录，使用key参数，删除所有使用allEntries参数(常用)
>
>本质：
>
>任然使用代理对象方式，当方法指向完毕后，才会操作缓存



使用位置：

* 新增、插入、编辑：为了保证数据一致，防止新增的内容缓存中有
* 删除、批量删除：保持数据一致，删除时要刷新缓存
* 启用停用：及时刷新缓存

**一般数据库有变化就要进行缓存删除，是删除单个还是全部，需要看复杂度，过于复杂直接全部删除**
