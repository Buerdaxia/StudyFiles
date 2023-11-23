# 配置properties到yml文件中

## 用途和好处

有时候有一些固定的变量，我们想要统一进行管理，然后统一管理的绝佳好地，就是项目的`yml`配置文件中，我们可以结合spring框架提供的`@ConfigurationProperties`注解来帮助我们读取到`yml`中的参数



好处:

第一统一管理，第二方便设置



核心：

1. `@ConfigurationProperties`注解
2. `yml`配置文件





## 具体方式

下面示例是关于`jwt`的一个示例，其他想要统一管理的例如阿里云的`oss`账号密码呀等等都可以这样使用

### 第一步：创建properties类

我们一般会创建一个类，并且交给IOC管理，到时候在想要使用的地方直接注入就可以直接用了



`properties/JwtProperties.class`

```java
package com.sky.properties;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "sky.jwt") // perfix指明是yml哪里的数据可以看下面yml
@Data
public class JwtProperties {

    /**
     * 管理端员工生成jwt令牌相关配置
     */
    private String adminSecretKey; // 注意这里要和yml文件中的对应
    private long adminTtl;
    private String adminTokenName;

    /**
     * 用户端微信用户生成jwt令牌相关配置
     */
    private String userSecretKey;
    private long userTtl;
    private String userTokenName;

}

```





### 第二步：配置`yml`

`application.yml`

```yaml
sky:
  jwt:
    # 设置jwt签名加密时使用的秘钥
    admin-secret-key: itcast
    # 设置jwt过期时间
    admin-ttl: 7200000
    # 设置前端传递过来的令牌名称
    admin-token-name: token
```

**这里的值要和上面properties类中的值对应**





### 第三步：使用

到指定想要使用的地方，直接`@Autowired`注入然后再直接调用getter方法就行辣



示例：

`controller/EmployeeController.class`

```java
@RestController
@RequestMapping("/admin/employee")
@Slf4j
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;
    @Autowired
    private JwtProperties jwtProperties; // 注入刚刚写好的jwtProperties类

    /**
     * 登录
     *
     * @param employeeLoginDTO
     * @return
     */
    @PostMapping("/login")
    public Result<EmployeeLoginVO> login(@RequestBody EmployeeLoginDTO employeeLoginDTO) {
        log.info("员工登录：{}", employeeLoginDTO);

        Employee employee = employeeService.login(employeeLoginDTO);

        //登录成功后，生成jwt令牌
        Map<String, Object> claims = new HashMap<>();
        claims.put(JwtClaimsConstant.EMP_ID, employee.getId());
      	// 看这里，下面直接调用getter方法获取
        String token = JwtUtil.createJWT(
                jwtProperties.getAdminSecretKey(),
                jwtProperties.getAdminTtl(),
                claims);

        EmployeeLoginVO employeeLoginVO = EmployeeLoginVO.builder()
                .id(employee.getId())
                .userName(employee.getUsername())
                .name(employee.getName())
                .token(token)
                .build();

        return Result.success(employeeLoginVO);
    }
}
```

