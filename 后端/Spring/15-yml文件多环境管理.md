# `yml`文件多环境管理

在真实的开发环境下，一般通常拥有多个环境例如开发环境`dev`，生产环境`prod`，测试环境`test`等等，所以`springboot`项目下之创建一个`yml`来进行项目配置往往比较麻烦





spring框架贴心的为我们提供了，在一个yml文件中，去引入另一个yml文件，所以我们通过这个就可以实现多环境下的`yml`管理



核心参数：

```yaml
# 引入
spring:
  profiles:
    active: dev
    
    
# 使用参数
${xxxx}
```





## 具体步骤

第一步，我们可以创建三个`yml`文件：

一个`application.yml`

另一个`application-dev.yml`

另另一个`application-prod.yml`



`application.yml`spring自带的配置文件：；

```yaml
server:
  port: 9090

# 关键在这里，如果我们要切换环境，直接修改active后面的内容就行
spring:
  profiles:
    active: dev # dev开发 prod生产
  main:
    allow-circular-references: true
  datasource:
    druid:
      driver-class-name: ${sky.datasource.driver-class-name}
      url: jdbc:mysql://${sky.datasource.host}:${sky.datasource.port}/${sky.datasource.database}?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&useSSL=false&allowPublicKeyRetrieval=true
      username: ${sky.datasource.username}
      password: ${sky.datasource.password}

mybatis:
  #mapper配置文件
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.sky.entity
  configuration:
    #开启驼峰命名
    map-underscore-to-camel-case: true

logging:
  level:
    com:
      sky:
        mapper: debug
        service: info
        controller: info

sky:
  jwt:
    # 设置jwt签名加密时使用的秘钥
    admin-secret-key: itcast
    # 设置jwt过期时间
    admin-ttl: 7200000
    # 设置前端传递过来的令牌名称
    admin-token-name: token
  alioss:
    access-key-id: ${sky.alioss.access-key-id}
    access-key-secret: ${access-key-secret}
    endpoint: ${sky.alioss.endpoint}
    bucket-name: ${sky.alioss.bucket-name}

```



`application-dev.yml`：

```yaml
# 开发环境数据库以及oss配置
sky:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    host: localhost
    port: 3306
    database: sky_take_out
    username: root
    password: 1234
  alioss:
    access-key-id: LTAI5t93P4VHMSKVL1Zr5GkE
    access-key-secret: oeCIU2EezqFdPzOMx9kLqgqxobnsJW
    endpoint: https://oss-cn-beijing.aliyuncs.com
    bucket-name: qianbuer-sky-bucket
```



`application-prod.yml`：

```yaml
# prod环境数据库以及oss配置生产环境
sky:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    host: xxxxx
    port: xxxx
    database: sky_take_out
    username: prod
    password: 123456
  alioss:
    access-key-id: LTAI5t93P4VHMSKVL1Zr5GkE
    access-key-secret: oeCIU2EezqFdPzOMx9kLqgqxobnsJW
    endpoint: https://oss-cn-beijing.aliyuncs.com
    bucket-name: qianbuer-sky-bucket
```

