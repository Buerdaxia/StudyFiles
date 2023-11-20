# 过滤器Filter和拦截器Interceptor

### 2.4 过滤器Filter

刚才通过浏览器的开发者工具，我们可以看到在后续的请求当中，都会在请求头中携带JWT令牌到服务端，而服务端需要统一拦截所有的请求，从而判断是否携带的有合法的JWT令牌。
那怎么样来统一拦截到所有的请求校验令牌的有效性呢？这里我们会学习两种解决方案：

1. Filter过滤器
2. Interceptor拦截器

我们首先来学习过滤器Filter。



#### 2.4.1 快速入门

什么是Filter？

- Filter表示过滤器，是 JavaWeb三大组件(Servlet、Filter、Listener)之一。
- 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能
  - 使用了过滤器之后，要想访问web服务器上的资源，必须先经过滤器，过滤器处理完毕之后，才可以访问对应的资源。
- 过滤器一般完成一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等。

![image-20230112120955145](assets/image-20230112120955145.png) 



下面我们通过Filter快速入门程序掌握过滤器的基本使用操作：

- 第1步，定义过滤器 ：1.定义一个类，实现 Filter 接口，并重写其所有方法。
- 第2步，配置过滤器：Filter类上加 @WebFilter 注解，配置拦截资源的路径。引导类上加 @ServletComponentScan 开启Servlet组件支持。



**定义过滤器**

创建：`filter/DemoFilter`

~~~java
/**
	
	注意Filter接口导包时候别导入错了，老版本是javax.servlet
	新版本是：jakarta.servlet
*/

//定义一个类，实现一个标准的Filter过滤器的接口
public class DemoFilter implements Filter {
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }

    @Override //拦截到请求之后调用, 调用多次
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Demo 拦截到了请求...放行前逻辑");
        //放行
        chain.doFilter(request,response);
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
~~~

> - init方法：过滤器的初始化方法。在web服务器启动的时候会自动的创建Filter过滤器对象，在创建过滤器对象的时候会自动调用init初始化方法，这个方法只会被调用一次。
>
> - doFilter方法：这个方法是在每一次拦截到请求之后都会被调用，所以这个方法是会被调用多次的，每拦截到一次请求就会调用一次doFilter()方法。
>
> - destroy方法： 是销毁的方法。当我们关闭服务器的时候，它会自动的调用销毁方法destroy，而这个销毁方法也只会被调用一次。



在定义完Filter之后，Filter其实并不会生效，还需要完成Filter的配置，Filter的配置非常简单，只需要在Filter类上添加一个注解：`@WebFilter`，并指定属性`urlPatterns`，通过这个属性指定过滤器要拦截哪些请求

~~~java
@WebFilter(urlPatterns = "/*") //配置过滤器要拦截的请求路径（ /* 表示拦截浏览器的所有请求 ）
public class DemoFilter implements Filter {
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }

    @Override //拦截到请求之后调用, 调用多次
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Demo 拦截到了请求...放行前逻辑");
        //放行
        chain.doFilter(request,response);
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
~~~

当我们在Filter类上面加了`@WebFilter`注解之后，接下来我们还需要在**启动类**上面加上一个注解`@ServletComponentScan`，通过这个`@ServletComponentScan`注解来开启SpringBoot项目对于Servlet组件的支持。**

~~~java
@ServletComponentScan
@SpringBootApplication
public class TliasWebManagementApplication {

    public static void main(String[] args) {
        SpringApplication.run(TliasWebManagementApplication.class, args);
    }

}
~~~



重新启动服务，打开浏览器，执行部门管理的请求，可以看到控制台输出了过滤器中的内容：

![image-20230112121205697](assets/image-20230112121205697.png) 



> 注意事项：
>
> ​	在过滤器Filter中，如果不执行放行操作，将无法访问后面的资源。 放行操作：chain.doFilter(request, response);



现在我们已完成了Filter过滤器的基本使用，下面我们将学习Filter过滤器在使用过程中的一些细节。





#### 2.4.2 Filter详解

Filter过滤器的快速入门程序我们已经完成了，接下来我们就要详细的介绍一下过滤器Filter在使用中的一些细节。主要介绍以下3个方面的细节：

1. 过滤器的执行流程
2. 过滤器的拦截路径配置
3. 过滤器链



##### 2.4.2.1 执行流程

首先我们先来看下过滤器的执行流程：

![image-20230106222559935](assets/image-20230106222559935.png)



过滤器当中我们拦截到了请求之后，如果希望继续访问后面的web资源，就要执行放行操作，**放行就是调用 FilterChain对象当中的doFilter()方法，在调用doFilter()这个方法之前所编写的代码属于放行之前的逻辑。**

在放行后访问完 web 资源之后还会回到过滤器当中，回到过滤器之后如有需求还可以执行放行之后的逻辑，放行之后的逻辑我们写在doFilter()这行代码之后。



~~~java
@WebFilter(urlPatterns = "/*") 
public class DemoFilter implements Filter {
    
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }
    
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
        
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
~~~

![image-20230106224322625](assets/image-20230106224322625.png)



##### 2.4.2.2 拦截路径

执行流程我们搞清楚之后，接下来再来介绍一下过滤器的拦截路径，Filter可以根据需求，配置不同的拦截资源路径：

| 拦截路径     | urlPatterns值 | 含义                                         |
| ------------ | ------------- | -------------------------------------------- |
| 拦截具体路径 | /login        | 只有访问 /login 路径时，才会被拦截           |
| 目录拦截     | /emps/*       | 访问/emps下的所有资源(包括/emps)，都会被拦截 |
| 拦截所有     | /*            | 访问所有资源，都会被拦截                     |

下面我们来测试"拦截具体路径"：

~~~java
@WebFilter(urlPatterns = "/login")  //拦截/login具体路径
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
    }


    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
~~~



测试1：访问部门管理请求，发现过滤器没有拦截请求

![image-20230106225658525](assets/image-20230106225658525.png)

![image-20230106230332510](assets/image-20230106230332510.png)



测试2：访问登录请求/login，发现过滤器拦截请求

![image-20230106230520229](assets/image-20230106230520229.png)





下面我们来测试"目录拦截"：

~~~java
@WebFilter(urlPatterns = "/depts/*") //拦截所有以/depts开头，后面是什么无所谓
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
    }


    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
~~~



测试1：访问部门管理请求，发现过滤器拦截了请求

![image-20230106231144348](assets/image-20230106231144348.png)



测试2：访问登录请求/login，发现过滤器没有拦截请求

![image-20230106231220802](assets/image-20230106231220802.png)



##### 2.4.2.3 过滤器链

最后我们在来介绍下过滤器链，什么是过滤器链呢？所谓过滤器链指的是在一个web应用程序当中，可以配置多个过滤器，多个过滤器就形成了一个过滤器链。

![image-20230107084730393](assets/image-20230107084730393.png)

比如：在我们web服务器当中，定义了两个过滤器，这两个过滤器就形成了一个过滤器链。

而这个链上的过滤器在执行的时候会一个一个的执行，会先执行第一个Filter，放行之后再来执行第二个Filter，如果执行到了最后一个过滤器放行之后，才会访问对应的web资源。

访问完web资源之后，按照我们刚才所介绍的过滤器的执行流程，还会回到过滤器当中来执行过滤器放行后的逻辑，而在执行放行后的逻辑的时候，顺序是反着的。

先要执行过滤器2放行之后的逻辑，再来执行过滤器1放行之后的逻辑，最后在给浏览器响应数据。



以上就是当我们在web应用当中配置了多个过滤器，形成了这样一个过滤器链以及过滤器链的执行顺序。下面我们通过idea来验证下过滤器链。

验证步骤：

1. 在filter包下再来新建一个Filter过滤器类：AbcFilter
2. 在AbcFilter过滤器中编写放行前和放行后逻辑
3. 配置AbcFilter过滤器拦截请求路径为：/* 
4. 重启SpringBoot服务，查看DemoFilter、AbcFilter的执行日志

![image-20230107085552176](assets/image-20230107085552176.png)

**AbcFilter过滤器**

~~~java
@WebFilter(urlPatterns = "/*")
public class AbcFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Abc 拦截到了请求... 放行前逻辑");

        //放行
        chain.doFilter(request,response);

        System.out.println("Abc 拦截到了请求... 放行后逻辑");
    }
}
~~~

**DemoFilter过滤器**

~~~java
@WebFilter(urlPatterns = "/*") 
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
    }
}
~~~

打开浏览器访问登录接口：

![image-20230107090425999](assets/image-20230107090425999.png)



通过控制台日志的输出，大家发现AbcFilter先执行DemoFilter后执行，这是为什么呢？

**其实是和过滤器的类名有关系。**以注解方式配置的Filter过滤器，它的执行优先级是按时过滤器类名的自动排序确定的，**类名排名越靠前，优先级越高。**

假如我们想让DemoFilter先执行，怎么办呢？答案就是修改类名。



测试：修改AbcFilter类名为XbcFilter，运行程序查看控制台日志

~~~java
@WebFilter(urlPatterns = "/*")
public class XbcFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Xbc 拦截到了请求...放行前逻辑");

        //放行
        chain.doFilter(request,response);

        System.out.println("Xbc 拦截到了请求...放行后逻辑");
    }
}

~~~

![image-20230107093757050](assets/image-20230107093757050.png)

到此，关于过滤器的使用细节，我们已经全部介绍完毕了。







#### 2.4.3 登录校验-Filter

##### 2.4.3.1 分析

过滤器Filter的快速入门以及使用细节我们已经介绍完了，接下来最后一步，我们需要使用过滤器Filter来完成案例当中的登录校验功能。



![image-20230107095010089](assets/image-20230107095010089.png)

我们先来回顾下前面分析过的登录校验的基本流程：

- 要进入到后台管理系统，我们必须先完成登录操作，此时就需要访问登录接口login。

- 登录成功之后，我们会在服务端生成一个JWT令牌，并且把JWT令牌返回给前端，前端会将JWT令牌存储下来。
- 在后续的每一次请求当中，都会将JWT令牌携带到服务端，请求到达服务端之后，要想去访问对应的业务功能，此时我们必须先要校验令牌的有效性。
- 对于校验令牌的这一块操作，我们使用登录校验的过滤器，在过滤器当中来校验令牌的有效性。如果令牌是无效的，就响应一个错误的信息，也不会再去放行访问对应的资源了。如果令牌存在，并且它是有效的，此时就会放行去访问对应的web资源，执行相应的业务操作。



大概清楚了在Filter过滤器的实现步骤了，那在正式开发登录校验过滤器之前，我们思考两个问题：

1. 所有的请求，拦截到了之后，都需要校验令牌吗？
   - 答案：**登录请求例外**

2. 拦截到请求后，什么情况下才可以放行，执行业务操作？
   - 答案：**有令牌，且令牌校验通过(合法)；否则都返回未登录错误结果**



##### 2.4.3.2 具体流程

我们要完成登录校验，主要是利用Filter过滤器实现，而Filter过滤器的流程步骤：

![image-20230112122130564](assets/image-20230112122130564.png) 



基于上面的业务流程，我们分析出具体的操作步骤：

1. 获取请求url
2. 判断请求url中是否包含login，如果包含，说明是登录操作，放行
3. 获取请求头中的令牌（token）
4. 判断令牌是否存在，如果不存在，返回错误结果（未登录）
5. 解析token，如果解析失败，返回错误结果（未登录）
6. 放行



##### 2.4.3.3 代码实现

分析清楚了以上的问题后，我们就参照接口文档来开发登录功能了，登录接口描述如下：



- 基本信息

  ~~~
  请求路径：/login
  
  请求方式：POST
  
  接口描述：该接口用于员工登录Tlias智能学习辅助系统，登录完毕后，系统下发JWT令牌。 
  ~~~

- 请求参数

  参数格式：application/json

  参数说明：

  | 名称     | 类型   | 是否必须 | 备注   |
  | -------- | ------ | -------- | ------ |
  | username | string | 必须     | 用户名 |
  | password | string | 必须     | 密码   |

  请求数据样例：

  ~~~json
  {
  	"username": "jinyong",
      "password": "123456"
  }
  ~~~

- 响应数据

  参数格式：application/json

  参数说明：

  | 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
  | ---- | ------ | -------- | ------ | ------------------------ | -------- |
  | code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
  | msg  | string | 非必须   |        | 提示信息                 |          |
  | data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

  响应数据样例：

  ~~~json
  {
    "code": 1,
    "msg": "success",
    "data": "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoi6YeR5bq4IiwiaWQiOjEsInVzZXJuYW1lIjoiamlueW9uZyIsImV4cCI6MTY2MjIwNzA0OH0.KkUc_CXJZJ8Dd063eImx4H9Ojfrr6XMJ-yVzaWCVZCo"
  }
  ~~~

- 备注说明

  用户登录成功后，系统会自动下发JWT令牌，然后在后续的每次请求中，都需要在请求头header中携带到服务端，请求头的名称为 token ，值为 登录时下发的JWT令牌。

  如果检测到用户未登录，则会返回如下固定错误信息：

  ~~~json
  {
  	"code": 0,
  	"msg": "NOT_LOGIN",
  	"data": null
  }
  ~~~



**登录校验过滤器：LoginCheckFilter**

~~~java
@Slf4j
@WebFilter(urlPatterns = "/*") //拦截所有请求 urlPatterns设置拦截请求路径
public class LoginCheckFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain chain) throws IOException, ServletException {
        //前置：强制转换为http协议的请求对象、响应对象 （转换原因：要使用子类中特有方法）
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //1.获取请求url
        String url = request.getRequestURL().toString();
        log.info("请求路径：{}", url); //请求路径：http://localhost:8080/login


        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行
        if(url.contains("/login")){
            chain.doFilter(request, response);//放行请求
            return;//结束当前方法的执行
        }


        //3.获取请求头中的令牌（token）
        String token = request.getHeader("token");
        log.info("从请求头中获取的令牌：{}",token);


        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasLength(token)){
            log.info("Token不存在");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）
        try {
            JwtUtils.parseJWT(token);
        }catch (Exception e){
            log.info("令牌解析失败!");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类 没有需要取prom中引入一下)
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }


        //6.放行
        chain.doFilter(request, response);

    }
}
~~~

在上述过滤器的功能实现中，我们使用到了一个第三方json处理的工具包fastjson。我们要想使用，需要引入如下依赖：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```



登录校验的过滤器我们编写完成了，接下来我们就可以重新启动服务来做一个测试：

> 测试前先把之前所编写的测试使用的过滤器，暂时注释掉。直接将@WebFilter注解给注释掉即可。

- 测试1：未登录是否可以访问部门管理页面

  首先关闭浏览器，重新打开浏览器，在地址栏中输入：http://localhost:9528/#/system/dept

  由于用户没有登录，登录校验过滤器返回错误信息，前端页面根据返回的错误信息结果，自动跳转到登录页面了

  ![image-20230105085212629](assets/image-20230105085212629.png)

  

- 测试2：先进行登录操作，再访问部门管理页面

  登录校验成功之后，可以正常访问相关业务操作页面

  ![image-20230107102922550](assets/image-20230107102922550.png)

 







### 2.5 拦截器Interceptor

学习完了过滤器Filter之后，接下来我们继续学习拦截器Interseptor。

拦截器我们主要分为三个方面进行讲解：

1. 介绍下什么是拦截器，并通过快速入门程序上手拦截器
2. 拦截器的使用细节
3. 通过拦截器Interceptor完成登录校验功能

我们先学习第一块内容：拦截器快速入门



#### 2.5.1 快速入门

什么是拦截器？

- 是一种动态拦截方法调用的机制，类似于过滤器。
- **拦截器是Spring框架中提供的**，用来动态拦截控制器方法的执行。

拦截器的作用：

- 拦截请求，在指定方法调用前后，根据业务需要执行预先设定的代码。



在拦截器当中，我们通常也是做一些通用性的操作，比如：我们可以通过拦截器来拦截前端发起的请求，将登录校验的逻辑全部编写在拦截器当中。在校验的过程当中，如发现用户登录了(携带JWT令牌且是合法令牌)，就可以直接放行，去访问spring当中的资源。如果校验时发现并没有登录或是非法令牌，就可以直接给前端响应未登录的错误信息。



下面我们通过快速入门程序，来学习下拦截器的基本使用。拦截器的使用步骤和过滤器类似，也分为两步：

1. 定义拦截器

2. 注册配置拦截器

   

**自定义拦截器：**实现HandlerInterceptor接口，并重写其所有方法

创建：`interceptor/LoginCheckInterceptor`

~~~java
//自定义拦截器
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    //目标资源方法执行前执行（就是在controller方法执行之前执行）。 返回true：放行    返回false：不放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
        
        return true; //true表示放行
    }

    //目标资源方法执行后执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ... ");
    }

    //视图渲染完毕后执行，最后执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion .... ");
    }
}
~~~



**注意：**

> ​	preHandle方法：目标资源方法执行前执行。 返回true：放行    返回false：不放行
>
> ​	postHandle方法：目标资源方法执行后执行
>
> ​	afterCompletion方法：视图渲染完毕后执行，最后执行



**注册配置拦截器**：实现WebMvcConfigurer接口，并重写addInterceptors方法

创建`config/WebConfig`

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //自定义的拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       	//注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
    }
}
~~~



重新启动SpringBoot服务，打开postman测试：

![image-20230107105224741](assets/image-20230107105224741.png)

![image-20230107105415120](assets/image-20230107105415120.png)



接下来我们再来做一个测试：将拦截器中返回值改为false

使用postman，再次点击send发送请求后，没有响应数据，说明请求被拦截了没有放行

![image-20230107105815511](assets/image-20230107105815511.png)









#### 2.5.2 Interceptor详解

拦截器的入门程序完成之后，接下来我们来介绍拦截器的使用细节。拦截器的使用细节我们主要介绍两个部分：

1. 拦截器的拦截路径配置
2. 拦截器的执行流程



##### 2.5.2.1 拦截路径

首先我们先来看拦截器的拦截路径的配置，在注册配置拦截器的时候，我们要指定拦截器的拦截路径，通过`addPathPatterns("要拦截路径")`方法，就可以指定要拦截哪些资源。

在入门程序中我们配置的是`/**`，表示拦截所有资源，而在配置拦截器时，不仅可以指定要拦截哪些资源，还可以指定不拦截哪些资源，只需要调用`excludePathPatterns("不拦截路径")`方法，指定哪些资源不需要拦截。

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**")//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
                .excludePathPatterns("/login");//设置不拦截的请求路径
    }
}
~~~



在拦截器中除了可以设置`/**`拦截所有资源外，还有一些**常见拦截路径设置：**

| 拦截路径  | 含义                 | 举例                                                |
| --------- | -------------------- | --------------------------------------------------- |
| /*        | 一级路径             | 能匹配/depts，/emps，/login，不能匹配 /depts/1      |
| /**       | 任意级路径           | 能匹配/depts，/depts/1，/depts/1/2                  |
| /depts/*  | /depts下的一级路径   | 能匹配/depts/1，不能匹配/depts/1/2，/depts          |
| /depts/** | /depts下的任意级路径 | 能匹配/depts，/depts/1，/depts/1/2，不能匹配/emps/1 |



下面主要来演示下`/**`与`/*`的区别： 

- 修改拦截器配置，把拦截路径设置为`/*`

~~~java
@Configuration 
public class WebConfig implements WebMvcConfigurer {

    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/*")
                .excludePathPatterns("/login");//设置不拦截的请求路径
    }
}
~~~

使用postman测试：http://localhost:8080/emps/1 

![image-20230107111525558](assets/image-20230107111525558.png)

控制台没有输出拦截器中的日志信息，说明`/*`没有匹配到拦截路径`/emp/1` 。

![image-20230107111812963](assets/image-20230107111812963.png)







##### 2.5.2.2 执行流程

介绍完拦截路径的配置之后，接下来我们再来介绍拦截器的执行流程。通过执行流程，大家就能够清晰的知道过滤器与拦截器的执行时机。

![image-20230107112136151](assets/image-20230107112136151.png)

- 当我们打开浏览器来访问部署在web服务器当中的web应用时，此时我们所定义的过滤器会拦截到这次请求。拦截到这次请求之后，它会先执行放行前的逻辑，然后再执行放行操作。而由于我们当前是基于springboot开发的，所以放行之后是进入到了spring的环境当中，也就是要来访问我们所定义的controller当中的接口方法。

- Tomcat并不识别所编写的Controller程序，但是它识别Servlet程序，所以在Spring的Web环境中提供了一个非常核心的Servlet：DispatcherServlet（前端控制器），所有请求都会先进行到DispatcherServlet，再将请求转给Controller。

- 当我们定义了拦截器后，会在执行Controller的方法之前，请求被拦截器拦截住。执行`preHandle()`方法，这个方法执行完成后需要返回一个布尔类型的值，如果返回true，就表示放行本次操作，才会继续访问controller中的方法；如果返回false，则不会放行（controller中的方法也不会执行）。

- 在controller当中的方法执行完毕之后，再回过来执行`postHandle()`这个方法以及`afterCompletion()` 方法，然后再返回给DispatcherServlet，最终再来执行过滤器当中放行后的这一部分逻辑的逻辑。执行完毕之后，最终给浏览器响应数据。



接下来我们就来演示下过滤器和拦截器同时存在的执行流程：

- 开启LoginCheckInterceptor拦截器

~~~java
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
        
        return true; //true表示放行
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ... ");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion .... ");
    }
}
~~~

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**")//拦截所有请求
                .excludePathPatterns("/login");//不拦截登录请求
    }
}
~~~



- 开启DemoFilter过滤器

~~~java
@WebFilter(urlPatterns = "/*") 
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
    }
}
~~~



重启SpringBoot服务后，清空日志，打开Postman，测试查询部门：

![image-20230107113653871](assets/image-20230107113653871.png)

![image-20230107114008004](assets/image-20230107114008004.png)





以上就是拦截器的执行流程。通过执行流程分析，大家应该已经清楚了过滤器和拦截器之间的区别，其实它们之间的区别主要是两点：

- 接口规范不同：过滤器需要实现Filter接口，而拦截器需要实现HandlerInterceptor接口。
- 拦截范围不同：过滤器Filter会拦截所有的资源，而Interceptor只会拦截Spring环境中的资源。





#### 2.5.3 登录校验- Interceptor

讲解完了拦截器的基本操作之后，接下来我们需要完成最后一步操作：通过拦截器来完成案例当中的登录校验功能。



登录校验的业务逻辑以及操作步骤我们前面已经分析过了，和登录校验Filter过滤器当中的逻辑是完全一致的。现在我们只需要把这个技术方案由原来的过滤器换成拦截器interceptor就可以了。



**登录校验拦截器**

~~~java
//自定义拦截器
@Component //当前拦截器对象由Spring创建和管理
@Slf4j
public class LoginCheckInterceptor implements HandlerInterceptor {
    //前置方式
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
        //1.获取请求url
        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行

        //3.获取请求头中的令牌（token）
        String token = request.getHeader("token");
        log.info("从请求头中获取的令牌：{}",token);

        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasLength(token)){
            log.info("Token不存在");

            //创建响应结果对象
            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            //设置响应头（告知浏览器：响应的数据类型为json、响应的数据编码表为utf-8）
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return false;//不放行
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）
        try {
            JwtUtils.parseJWT(token);
        }catch (Exception e){
            log.info("令牌解析失败!");

            //创建响应结果对象
            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            //设置响应头
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return false;
        }

        //6.放行
        return true;
    }
~~~

**注册配置拦截器**

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {
    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/login");
    }
}

~~~



登录校验的拦截器编写完成后，接下来我们就可以重新启动服务来做一个测试： （**关闭登录校验Filter过滤器**）

- 测试1：未登录是否可以访问部门管理页面

  首先关闭浏览器，重新打开浏览器，在地址栏中输入：http://localhost:9528/#/system/dept

  由于用户没有登录，校验机制返回错误信息，前端页面根据返回的错误信息结果，自动跳转到登录页面了

  ![image-20230105085212629](assets/image-20230105085212629.png)

  

- 测试2：先进行登录操作，再访问部门管理页面

  登录校验成功之后，可以正常访问相关业务操作页面

  ![image-20230107102922550](assets/image-20230107102922550.png)

到此我们也就验证了所开发的登录校验的拦截器也是没问题的。登录校验的过滤器和拦截器，我们只需要使用其中的一种就可以了。





