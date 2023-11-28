# Java的注解

## 简介

Java注解时附加在代码中的一些**元信息**，用于一些工具在编译、运行进行解析和使用，**起到说明、配置的功能**，注解的相关类都是在java.lang.annotation包中。



## 注解分类



### 一：JDK基本注解

```java
@Override
重写
 
@SuppressWarnings(value = "unchecked")
压制编辑器警告
```





### 二：JDK元注解

```java
@Retention：定义注解的保留策略
@Retention(RetentionPolicy.SOURCE)             //注解仅存在于源码中，在class字节码文件中不包含
@Retention(RetentionPolicy.CLASS)              //默认的保留策略，注解会在class字节码文件中存在，但运行时无法获得，
@Retention(RetentionPolicy.RUNTIME)            //注解会在class字节码文件中存在，在运行时可以通过反射获取到
 
@Target：指定被修饰的Annotation可以放置的位置(被修饰的目标)
@Target(ElementType.TYPE)                      //接口、类
@Target(ElementType.FIELD)                     //属性
@Target(ElementType.METHOD)                    //方法
@Target(ElementType.PARAMETER)                 //方法参数
@Target(ElementType.CONSTRUCTOR)               //构造函数
@Target(ElementType.LOCAL_VARIABLE)            //局部变量
@Target(ElementType.ANNOTATION_TYPE)           //注解
@Target(ElementType.PACKAGE)                   //包
注：可以指定多个位置，例如：
@Target({ElementType.METHOD, ElementType.TYPE})，也就是此注解可以在方法和类上面使用
 
@Inherited：指定被修饰的Annotation将具有继承性
 
@Documented：指定被修饰的该Annotation可以被javadoc工具提取成文档.
```





### 三：自定义注解

注解分类（根据Annotation是否包含成员变量,可以把Annotation分为两类）:

标记Annotation:
没有成员变量的Annotation; 这种Annotation仅利用自身的存在与否来提供信息

元数据Annotation:
包含成员变量的Annotation; 它们可以接受(和提供)更多的元数据;





## 手动自定义注解(重点)

自定义注解通过`@interface`关键字进行定义，定义过程和接口非常相似不过需要注意下面几点：

1. **Annotation中的成员变量是以无参的方法来进行声明的，其方法名和返回值类型定义了该成员变量的名字和类型**
2. 还可以通过default关键字来指定这个成员变量的默认值
3. 一般需要加上`@Target`来指定注解的使用位置和`@Retention`来定义注解保留策略(大多都是`RetentionPolicy.RUNTIME`)



### 定义注解

简单示例：

`annotation/AutoFill`

```java
package com.sky.annotation;

import com.sky.enumeration.OperationType;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * 自定义注解：用于标识某个方法是否需要进行功能字段自动处理
 */
@Target(ElementType.METHOD) // 标记是在方法上使用
@Retention(RetentionPolicy.RUNTIME) // 标记保留策略，字节码和运行时都能反射获取
public @interface AutoFill {
    String name();
    int age();
}

```



### 使用注解

```java
@MyAnnotation(name="张三",age=18)
    public class MyClass {
        //类定义
    }
```





###　处理注解

使用注解后，我们直接通过反射机制来获取到注解以及其相关的属性值：

```java
MyAnnotation myAnnotation = MyClass.class.getAnnotation(MyAnnotation.class);
String name = myAnnotation.name();
int age = myAnnotation.age();
```

在这个例子中，我们使用反射机制获取MyClass类上的MyAnnotation注解，并获取其属性的值。

总之，自定义注解可以让我们更好地标记代码，方便进行特定的处理。在使用注解时，需要注意注解的生命周期和应用场景，这样才能更好地发挥注解的作用。