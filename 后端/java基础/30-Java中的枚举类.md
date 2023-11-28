# Java中的枚举类

## 简介：

枚举类型是一种**特殊的数据类型**，用于**定义一组固定的命名常量**。枚举类型提供了一种更强大、更安全和更易读的方式来表示一组相关的常量。

在Java中，枚举类型是通过使用`enum`关键字来定义的。枚举类型可以包含一个或多个枚举常量，每个常量都是枚举类型的实例。**枚举常量是在枚举类型中预先定义的，它们是唯一的、已命名的对象**。



## 特点：

1. 有限的实例集合：枚举类型是一种有限的实例集合，每个实例都是该枚举类型的一个唯一的、已命名的常量。枚举类型的实例在定义时就被预先确定，并且不可修改。

2. 类型安全：枚举类型在编译时进行静态类型检查，这意味着编译器可以检测到在使用枚举常量时的类型错误。这提供了更高的类型安全性，避免了一些常见的编程错误。

3. 唯一性和可比性：每个枚举常量在枚举类型中都是唯一的，并且可以使用==操作符进行比较。这使得可以在代码中使用枚举常量来进行精确的比较和判断。

4. 可读性和可维护性：枚举类型的常量是有意义的、自描述的，可以直观地理解其含义。这使得代码更易读、易理解和易于维护。同时，枚举类型的常量也可以提供更好的文档和注释。

5. 可迭代性：枚举类型可以通过values()方法获取包含所有枚举常量的数组，并且支持for-each循环遍历。这使得可以方便地对枚举常量进行迭代和处理。

6. 支持方法和字段：枚举常量可以具有字段和方法，可以为每个常量定义特定的属性和行为。这使得可以将相关的属性和操作封装在枚举常量内部。

7. 序列化支持：枚举类型默认实现了Serializable接口，可以被序列化和反序列化。这使得可以在网络传输、存储和持久化等场景下使用枚举类型。

通过利用枚举类型，可以更优雅地表示固定的命名常量集合，并提供更好的类型安全性和代码可读性。它们在很多场景下都可以提供更简洁、可维护和可扩展的解决方案。






##　枚举类型的使用

### 创建语法

在Java中枚举的简单创建方式：

```java
enum EnumName {
    CONSTANT1,
    CONSTANT2
}
```

其中，`EnumName`是枚举类型的名称，`CONSTANT1`、`CONSTANT2`等是枚举常量，用逗号分隔。每个枚举常量都是枚举类型的实例，是唯一的、已命名的常量对象。

注意：**枚举在创建时就一定规定好了里面的常量对象，不能再创建新的了**





### 枚举类型比较

由于枚举在创建时就已经定义好了实例，所以我们可以直接那实例通过`==`判断符号来进行比较。

示例：

```java
public enum test {
    RED,BLACK,GREEN,WHITE;
 
    public static void main(String[] args) {
        test value1 = test.BLACK;
        test value2 = test.RED;
        test value3 = test.RED;
        // 使用 运算符 '=='
        System.out.println(value1 == value2);
        // 使用equals()方法
        System.out.println(value1.equals(value2));
 
        System.out.println(value3 == value2);
    }
}
```

运行结果:

```java
false
false
true
```



### 简单使用场景

#### switch-case

```java
public enum TestEnum {
    RED,BLACK,GREEN,WHITE;
    public static void main(String[] args) {
        TestEnum testEnum2 = TestEnum.BLACK;
 
        switch (testEnum2) {
            case RED:
                System.out.println("red");
                break;
            case BLACK:
                System.out.println("black");
                break;
            case WHITE:
                System.out.println("WHITE");
                break;
            case GREEN:
                System.out.println("black");
                break;
            default:
                break;
        }
    }
}
```





### Enum类的常用方法

虽然enum类没有显示继承类, 但是其实底层默认继承了Enum类, Enum类中提供了一些方法,用来对这个enum类来进行一些便捷操作:

| **方法**    | **作用**                         |
| ----------- | -------------------------------- |
| values()    | 以数组形式返回枚举类型的所有成员 |
| ordinal()   | 获取枚举成员的索引位置           |
| valueOf()   | 将普通字符串转换为枚举实例       |
| compareTo() | 比较两个枚举成员在定义时的顺序   |
| toString()  | 以字符串的形式返回枚举常量名     |



#### values()

作用：以数组方式返回枚举类型中的所有成员



* 直接fori循环：

  ```java
  public enum test {
      RED,BLACK,GREEN,WHITE;
   
      public static void main(String[] args) {
          test[] tem = test.values();
   
          for(int i = 0; i < tem.length; i++) {
              System.out.println(tem[i]);
          }
      }
   
  }
  ```

  



#### ordinal()

作用：获取枚举成员的索引位置

例如现在有如下枚举类型：

```java
public enum test {RED,BLACK,GREEN,WHITE;}
```

它的索引类型默认是这样的：

RED : 0

BLACK: 1

GREEN: 2

WHITE: 3



示例：

```java
public enum test {
    RED,BLACK,GREEN,WHITE;
 
    public static void main(String[] args) {
        test value = test.WHITE;
        int ret = value.ordinal();
        System.out.println(ret); // 3
    }
}
```





####　valueOf()

作用：返回一个字符串对应的枚举示例

```java
public enum test {
    RED,BLACK,GREEN,WHITE;
 
    public static void main(String[] args) {
        String tem = "RED";
        //  test.class是返回一个test类的类型的实例
        test ret = Enum.valueOf(test.class,"RED");
        System.out.println(ret); // RED
    }
}
```



#### compareTo()

作用：简单理解为比较两个枚举成员在定义时的顺序，

示例：

```java
public enum test {
    RED,BLACK,GREEN,WHITE;
 
    public static void main(String[] args) {
        test value1 = test.BLACK;
        test value2 = test.WHITE;
        System.out.println(value1.compareTo(value2));
        // 输出-2， value1索引值为1 value2索引值为3 1 - 3 为 -2
    }
}
```





####　toString()

作用：将对应枚举常量转化为字符串的形式.

示例：

```java
public enum test {
    RED,BLACK,GREEN,WHITE;
 
    public static void main(String[] args) {
        test value = test.BLACK;
        String ret = value.toString();
        System.out.println(ret);
        // 字符串BLACK
    }
}
```







## 自定义枚举构造(重点)

在Java中`Enum`就是一个类，我们自然能够定义它的构造方法，还能写一些实例方法

>注意：当枚举实例有参数时，我们就必须要编写构造方法



示例：

```java
public class EnumTest {
    public static void main(String[] args) {
        ErrorCodeEnum errorCode = ErrorCodeEnum.SUCCESS;
        System.out.println("状态码：" + errorCode.code() + 
                           " 状态信息：" + errorCode.msg());
        // 状态码：1000 状态信息：success
    }
}
 
enum ErrorCodeEnum {
    SUCCESS(1000, "success"),
    PARAM_ERROR(1001, "parameter error"),
    SYS_ERROR(1003, "system error"),
    NAMESPACE_NOT_FOUND(2001, "namespace not found"),
    NODE_NOT_EXIST(3002, "node not exist"),
    NODE_ALREADY_EXIST(3003, "node already exist"),
    UNKNOWN_ERROR(9999, "unknown error");
 
    private int code;
    private String msg;
 
    ErrorCodeEnum(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }
 
    public int code() {
        return code;
    }
 
    public String msg() {
        return msg;
    }
 
    public static ErrorCodeEnum getErrorCode(int code) {
        for (ErrorCodeEnum it : ErrorCodeEnum.values()) {
            if (it.code() == code) {
                return it;
            }
        }
        return UNKNOWN_ERROR;
    }
}
```

