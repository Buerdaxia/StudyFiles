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

