# Stream流

##	简介

作用：结合Lambda表达式，用来简化集合、数组的操作



使用步骤：

1. 先得到一条Stream流，并把数据放上去
2. 使用**中间方法**对流水线上的数据进行操作
3. 使用**终结方法**对流水线上的数据进行操作



##  获取Stream流

![20-stream流的创建](./imgs/20-stream流的创建.png)

>tips：
>
>双列集合不能直接获得Stream流，需要通过keySet或entrySet方法获取Set集合后再获取



代码示例：

```java
package a02stream;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashMap;
import java.util.stream.Stream;

public class A01SteamDemo2 {
    public static void main(String[] args) {

        // 1.单列集合获取Stream流
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"a", "b", "c");
        // 1.1获取Stream + 调用终结方法forEach
        list.stream().forEach(s -> {
            System.out.println(s);
        });



        // 2.双列集合
        HashMap<String, Integer> hm = new HashMap<>();

        // 2.1添加数据
        hm.put("aaa", 111);
        hm.put("bbb", 222);
        hm.put("ccc", 333);

        // 2.2.获取Stream流
        hm.keySet().stream().forEach(s -> System.out.println(s)); // 所有键

        hm.entrySet().stream().forEach(o -> System.out.println(o)); // 所有键值对对象




        // 3.使用数组来获取Stream
        int[] arr = {1, 2, 3, 4, 5, 6};
        String[] arr2 = {"a", "b", "c"};
        Arrays.stream(arr).forEach(s -> System.out.println(s)); // 基本类型
        Arrays.stream(arr2).forEach(s -> System.out.println(s)); // 引用类型



        // 4.一堆零散的数据（要求：这堆数据的数据类型要一致）
        Stream.of(1, 2, 3, 4, 5).forEach(s -> System.out.println(s)); // 基本类型
        Stream.of("a", "b", "c", "d", "e").forEach(s -> System.out.println(s)); // 引用类型

    }
}

```

