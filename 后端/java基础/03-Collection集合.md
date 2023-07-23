# Collection集合



## 简述

首先：Collection属于单列集合

Collection集合概述

- 是单例集合的顶层接口,它表示一组对象,这些对象也称为Collection的元素
- JDK 不提供此接口的任何直接实现.它提供更具体的子接口(如Set和List)实现



具体构成：

![03-Collection集合体系](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合体系.PNG)





常见的相关方法：

![03-Collection集合方法](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合方法.PNG)

小细节：

1. `add()`细节：首先我们可以看到add方法的返回值是一个布尔型，但是其实List集合，add方法永远都返回的是true，应为List系列是可以允许有重复元素的，但是Set集合会有false返回值，当添加元素重复时就会返回false
2. `remove()`细节：remove必须指定对象去删除，这里为什么不用索引呢？应为Set是无序的不能直接用索引
3. `contains()`细节：底层是通过equals方法进行判断的，如果**存储自定义的对象**，想要使用contains方法，就必须要**重写`equals`方法**,否则使用的是`Object`初始的equals方法





代码示例：

```java
package com.itheima.mycollection;

import java.util.ArrayList;
import java.util.Collection;

public class A02_CollectionDemo2 {
    public static void main(String[] args) {
        // 1.创建集合对象
        Collection<Student> coll = new ArrayList<>();


        Student s1 = new Student("张三", 21);
        Student s2 = new Student("李四", 22);
        Student s3 = new Student("王五", 23);


        coll.add(s1);
        coll.add(s2);
        coll.add(s3);


        // 2.判断是否包含
        Student s4 = new Student("张三", 21);

        // contains本质，使用的是equals方法进行比较，如果没有重写，就会使用Object身上的equals，但是这个方法是比较地址值的
        System.out.println(coll.contains(s4));


        // 3.empty
        System.out.println(coll.isEmpty()); // false


        // 4.size
        System.out.println(coll.size()); // 3

        
        // 5.remove

        coll.remove(s1);
        System.out.println(coll); // 只剩下李四(s2)和王五(s3)了
    }
}

```







## Collection通用的遍历方式

### 迭代器遍历

迭代器在Java种类是Iterator，迭代器是集合的专用遍历方式

特点：

1. 迭代器遍历集合时，不依赖于索引





常用方法：

![03-Collection集合iterator](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合iterator.PNG)



遍历图解：

![03-Collection集合iterator遍历图解](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合iterator遍历图解.PNG)



代码示例：

```java
package com.itheima.mycollection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class A03_CollectionDemo3 {
    public static void main(String[] args) {
        // 1.创建集合对象
        Collection<Student> coll = new ArrayList<>();


        Student s1 = new Student("张三", 21);
        Student s2 = new Student("李四", 22);
        Student s3 = new Student("王五", 23);


        coll.add(s1);
        coll.add(s2);
        coll.add(s3);



        /*
            迭代器遍历相关的三个方法
            Iterator<E> iterator(): 获取一个迭代器对象
            boolean hasNext():      判断当前元素是否指向元素
            E next():               获取当前元素并移动指针
         */

        // 2.获取迭代器对象
        Iterator<Student> it = coll.iterator();
        

        // 3.循环遍历
        while (it.hasNext()) {
            Student s = it.next();

            System.out.println(s);
        }

    }
}

```





迭代器遍历的注意点：

1. 如果it.hasNext返回false了(指向最后没有元素的位置了)，再通过next获取元素时，就会报NoSuchElementException异常
2. **迭代器遍历完毕后，指针不会复位**（如果要第二次遍历的话，就必须再次获取一个迭代器注意，这是一个**新的迭代器了**）
3. 循环中只能调用一次`next`方法，这个不用说了吧，调用一次就移动一次，你多调用就会出问题的
4. **迭代器遍历时(注意是遍历这个过程中)**，不能用**集合的方法**进行增加或删除（如果调用add方法或remove方法，会有ConcurrentModificationException异常）**如果非要删除，需要调用迭代器自带的remove方法**，暂时还没有能添加的办法





### 增强for遍历

概述：

- 它是JDK5之后出现的,其内部原理是一个Iterator迭代器
- 实现Iterable接口的类才可以使用迭代器和增强for
- 简化数组和Collection集合的遍历

注意：

**所有的单列集合和数组才能用增强for进行遍历**



格式：

```java
for(集合/数组中元素的数据类型 变量名 :  集合/数组名) {

	// 已经将当前遍历到的元素封装到变量中了,直接使用变量即可

}
```



代码示例：

```java
public class MyCollectonDemo1 {
    public static void main(String[] args) {
        ArrayList<String> list =  new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("d");
        list.add("e");
        list.add("f");

        //1,数据类型一定是集合或者数组中元素的类型
        //2,str仅仅是一个变量名而已,在循环的过程中,依次表示集合或者数组中的每一个元素
        //3,list就是要遍历的集合或者数组
        for(String str : list){
            System.out.println(str);
        }
        
        // idea快捷生成方式   集合名.for + 回车
    }
}
```



小细节：

1. 修改增强for中的变量，不会改变集合中原本的数据

   ```java
   for(String str : list){
       System.out.println(str);
       // 注意这里str，是一个完全独立的第三方变量，和原本的元素完全是两个不一样的东西，可以理解为深拷贝出来的一份
   }
   ```

   



### Lambda表达式遍历

![03-Collection集合lambda表达式遍历](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合lambda表达式遍历.PNG)



代码示例：

```java
package com.itheima.mycollection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.function.Consumer;

public class A05_CollectionDemo5 {
    public static void main(String[] args) {
        // 1.创建集合对象
        Collection<Student> coll = new ArrayList<>();


        Student s1 = new Student("张三", 21);
        Student s2 = new Student("李四", 22);
        Student s3 = new Student("王五", 23);


        coll.add(s1);
        coll.add(s2);
        coll.add(s3);



        /*
          利用lambda表达式遍历

           底层：
           也就是自己遍历元素集合，依次得到每一元素，然后交给accept方法
         */


        // 先用匿名内部类方式写一下
        coll.forEach(new Consumer<Student>() {
            @Override
            public void accept(Student student) {
                // 打印
                System.out.println(student);
            }
        });

        System.out.println("-----------------------------");

        // 使用lambda表达式简化

        coll.forEach((s) -> {
            // 打印
            System.out.println(s);
        });
    }
}

```



### 三种遍历的使用时机

如果在删除过程中想要删除元素，那么请使用`iterator`遍历

如果只是想遍历一下，那么可以使用增强for或者lambda表达式遍历





## List系列

### 简述

特点：

1. 添加的元素时**有序、可重复、有索引**



>tips:
>
>有序：指的是存和取是有序的，例如存的是"张三"，"李四"，"王五"。取出来还是"张三"，"李四"，"王五"
>
>有索引：指可以通过索引操作元素
>
>可重复：可以存重复元素



List集合实现了Collection接口所以拥有Collection接口的方法，但自己由于拥有索引的存在，自己也拥有一些特殊方法：

![03-Collection集合List特有方法](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合List特有方法.PNG)

代码示例：

```java
package com.itheima.mycollection.a02mylist;

import java.util.ArrayList;
import java.util.List;

public class A01_ListDemo1 {
    public static void main(String[] args) {

        /*
            List独有的方法：
            void add(int index, E element)
            E remove(int index)
            E set(int index, E element)
            E get(int index)
         */


        // 1.创建一个集合 通过多态方式创建一个
        List<String> list = new ArrayList<>();

        list.add("111");
        list.add("222");
        list.add("333");


        // 2.add方法，
        // 细节：在指定索引index位置添加元素，位置上原来元素自动向后移动
        list.add(1, "AAA");


        // 3.remove方法
        String l = list.remove(1);
        System.out.println(l); // 返回被删除的元素 AAA

        System.out.println(list);


        // 4.set方法 设置元素
 
        String l2 = list.set(0, "000");
        System.out.println(l2); // 返回被修改的元素 111
        System.out.println(list); // [000, 222, 333]


        // 5.get方法 得到元素
        System.out.println(list.get(0)); // 000
    }
}

```





remove方法的小细节：

核心：**如果调用方法时，有重载现象，会优先调用，实参和形参一致的那个方法**

```java
package com.itheima.mycollection.a02mylist;

import java.util.ArrayList;
import java.util.List;

public class A02_ListDemo2 {
    public static void main(String[] args) {

        /*
            remove的小细节
         */

        List<Integer> list = new ArrayList<>();

        list.add(1);
        list.add(2);
        list.add(3);


        /*
            问题：remove是删除1索引上的元素，还是删除1这个元素呢？
            答案：会调用重写的remove删除1索引上的元素，

            原因：如果调用方法时，有重载现象，会优先调用，实参和形参一致的那个方法
         */
        list.remove(1);
        System.out.println(list); // [1, 3]

        /*
            那如何删除元素1呢？
            创建一个integer 1 对象，然后调用remove即可
         */

        Integer i = Integer.valueOf(1);

        list.remove(i);
        System.out.println(list); // [3]
    }
}

```





### List集合的遍历方式

#### 迭代器遍历

和Collection的一致

#### 列表迭代器遍历（特有）

列表迭代器接口：`ListIterator<E>`

和iterator几乎一模一样，有一个非常关键的add方法特有

![03-Collection集合List遍历listiterator](C:\Users\UserName\Desktop\web前端学习\StudyFiles\后端\java基础\imgs\03-Collection集合List遍历listiterator.PNG)

这里有几个比较有意思的方法，想previous和previousIndex和next，nextIndex刚好相反



特点：

1. iterator迭代器遍历中，时不允许在遍历过程中添加元素的，但是列表遍历器自带了一个add方法，它时可以进行添加元素的（在遍历过程中）





#### 增强for遍历

和Collection一致



#### lambda表达式

和Collection一致



#### 普通的for.i遍历

核心：

1. size方法
2. get方法
3. for循环

代码示例：

```java
package com.itheima.mycollection.a02mylist;

import java.util.ArrayList;
import java.util.List;

public class A03_ListDemo3 {
    public static void main(String[] args) {


        List<String> list = new ArrayList<>();

        list.add("aaa");
        list.add("bbb");
        list.add("ccc");


        // 普通for.i遍历

        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}

```





#### 五种迭代器使用时机

如果在遍历过程中要**删除元素**，请使用迭代器遍历

如果在遍历过程中要**添加元素**，请使用列表迭代器遍历

如果只是简单遍历请使用增强for或者lambda表达式

如果遍历时想要操作索引，可以使用普通for遍历







## Set集合



特点：

1. 添加的元素是无序、不重复、无索引



