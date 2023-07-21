# Lambda表达式

lambda可以帮助我们简化代码（**简化函数式接口的匿名内部类写法**）的好帮手🐂🐂🐂

本质：就是一个匿名函数

## 函数式编程（Functional programming）

函数式编程是一种思想特点。函数式编程思想，忽略面向对象的复杂语法，**强调做什么，而不是谁去做**。

Lambda表达式就可以说式一种函数式编程的体现





## Lambda格式

Lambda表达式是JDK 8开始后的一种新语法：

格式：

```java
() -> {
  
}
// 有点像JavaScript的箭头函数0，0
```



省略规则：

省略核心：可推导，可省略

1. 参数类型可以省略不写
2. 如果只有一个参数，参数类型可以省略不写，同时()也可以不写
3. 如果Lambda表达式方法体只有一行，可以不写{}、return、分号;（**必须三个这三个都不写才行**）

示例：

```java
// 通过Lambda表达式简写匿名内部类 原版
Arrays.sort(arr, (Integer o1, Integer o2) -> {
    return o1 - o2;
});

// 通过Lambda表达式简写匿名内部类 省略版本
Arrays.sort(arr, (o1, o2) -> o1 - o2);
```



## Lambda表达式注意点



1. lambda表达式可以用来简化匿名内部类的书写
2. lambda表达式**只能简化函数式接口**的匿名内部类的写法

到时候我们不确定能不能用`lambda`表达式，就可以看下源码，如果有`@FunctionalInterface`就可以用`lambda`表达式

>tips:
>
>函数式接口：
>
>有且只有一个抽象方法的接口叫做函数式接口，接口上方可以加@FunctionalInterface注解





## lambda小示例



示例1：利用lambda简写Array.sort方法



```java
package com.itheima.lambda;

import java.util.Arrays;
import java.util.Comparator;

public class LambdaDemo1 {
    public static void main(String[] args) {

        Integer[] arr = {2, 3, 5, 1, 4, 6, 7, 9, 8};


        // 原版sort写法
        Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });

        // 通过Lambda表达式简写匿名内部类
        Arrays.sort(arr, (Integer o1, Integer o2) -> {
            return o1 - o2;
        });
      
      
      	// 通过Lambda表达式简写匿名内部类 省略版本
        Arrays.sort(arr, (o1, o2) -> o1 - o2);
    }
}

```

