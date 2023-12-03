# static

# 第一章 复习回顾

## 1.1 如何定义类

类的定义格式如下:

```java
修饰符 class 类名 {
    // 1.成员变量（属性）
    // 2.成员方法 (行为) 
    // 3.构造方法 （初始化类的对象数据的）
}
```

例如:

```java
public class Student {
    // 1.成员变量
    public String name ;
    public char sex ; // '男'  '女'
    public int age;
}
```

## 1.2 如何通过类创建对象

```java
类名 对象名称 = new 类名();
```

例如:

```java
Student stu = new Student();
```

## 1.3 封装

#### 1.3.1 封装的步骤

1.使用 `private` 关键字来修饰成员变量。

2.使用`public`修饰getter和setter方法。

#### 1.3.2 封装的步骤实现

1. private修饰成员变量

```java
public class Student {
    private String name;
    private int age;
}
```

2. public修饰getter和setter方法

```java
public class Student {
    private String name;
    private int age;

    public void setName(String n) {
      	name = n;
    }

    public String getName() {
      	return name;
    }

    public void setAge(int a) {
        if (a > 0 && a <200) {
            age = a;
        } else {
            System.out.println("年龄非法！");
        }
    }

    public int getAge() {
      	return age;
    }
}
```

## 1.4 构造方法

### 1.4.1 构造方法的作用

在创建对象的时候，给成员变量进行初始化。

初始化即赋值的意思。

### 1.4.2 构造方法的格式

```java
修饰符 类名(形参列表) {
    // 构造体代码，执行代码
}
```

### 1.4.3 构造方法的应用

首先定义一个学生类，代码如下：

```java
public class Student {
    // 1.成员变量
    public String name;
    public int age;

    // 2.构造方法
    public Student() {
		System.out.println("无参数构造方法被调用")；
    }
}
```

接下来通过调用构造方法得到两个学生对象。

```java
public class CreateStu02 {
    public static void main(String[] args) {
        // 创建一个学生对象
        // 类名 变量名称 = new 类名();
        Student s1 = new Student();
        // 使用对象访问成员变量，赋值
        s1.name = "张三";
        s1.age = 20 ;

        // 使用对象访问成员变量 输出值
        System.out.println(s1.name);
        System.out.println(s1.age); 

        Student s2 = new Student();
        // 使用对象访问成员变量 赋值
        s2.name = "李四";
        s2.age = 18 ;
        System.out.println(s2.name);
        System.out.println(s2.age);
    }
}
```

## 1.5 this关键字的作用

### 1.5.1 this关键字的作用

this代表所在类的当前对象的引用（地址值），即代表当前对象。

### 1.5.2 this关键字的应用

#### 1.5.2.1 用于普通的gettter与setter方法

this出现在实例方法中，谁调用这个方法（哪个对象调用这个方法），this就代表谁（this就代表哪个对象）。

```java
public class Student {
    private String name;
    private int age;

    public void setName(String name) {
      	this.name = name;
    }

    public String getName() {
      	return name;
    }

    public void setAge(int age) {
        if (age > 0 && age < 200) {
            this.age = age;
        } else {
            System.out.println("年龄非法！");
        }
    }

    public int getAge() {
      	return age;
    }
}
```

#### 1.5.2.2 用于构造方法中

this出现在构造方法中，代表构造方法正在初始化的那个对象。

```java
public class Student {
    private String name;
    private int age;
    
    // 无参数构造方法
    public Student() {} 
    
    // 有参数构造方法
    public Student(String name,int age) {
    	this.name = name;
    	this.age = age; 
    }
}
```

# 第二章 static关键字   

## 2.1 概述

以前我们定义过如下类：

```java
public class Student {
    // 成员变量
    public String name;
    public char sex; // '男'  '女'
    public int age;

    // 无参数构造方法
    public Student() {

    }
    
    // 有参数构造方法
    public Student(String  a) {

    }
}
```

我们已经知道面向对象中，存在类和对象的概念，我们在类中定义了一些成员变量，例如name,age,sex ,结果发现这些成员变量，每个对象都存在（因为每个对象都可以访问）。

而像name ,age , sex确实是每个学生对象都应该有的属性，应该属于每个对象。

所以Java中成员（**变量和方法**）等是存在所属属性的(**属于谁？**)，Java是通过static关键字来区分的。**static关键字在Java开发非常的重要，对于理解面向对象非常关键。**

关于 `static` 关键字的使用，它可以用来修饰的成员变量和成员方法，被static修饰的成员是**属于类**的是放在**静态区**中，没有static修饰的成员变量和方法则是**属于对象**的。我们上面案例中的成员变量都是没有static修饰的，所以属于每个对象。



注意：

静态区：**静态区是堆中一块儿独立的区域**



static关键字修饰的特点：

* **被改类创建的所有实例共享**
* 优先被创建，随着类的创建而创建(注意：**比实例对象创建的要早**)



## 2.2 定义格式和使用 

static是静态的意思。**static可以修饰成员变量或者修饰方法。**



### 2.2.1 静态变量及其访问

有static修饰成员变量，说明这个成员变量是属于类的，这个成员变量称为**类变量**或者**静态成员变量**。 直接用  类名访问即可。因为类只有一个，所以静态成员变量在内存区域中也只存在一份。所有的对象都可以共享这个变量。

**如何使用呢**

例如现在我们需要定义传智全部的学生类，那么这些学生类的对象的学校属性应该都是“传智”，这个时候我们可以把这个属性定义成static修饰的静态成员变量。

**定义格式**

```java
修饰符 static 数据类型 变量名 = 初始值；    
```

**举例**

```java
public class Student {
    public static String schoolName = "传智播客"； // 属于类，只有一份。
    // .....
}
```

**静态成员变量的访问（推荐的使用方式）:**

**格式：类名.静态变量**

```java
public static void  main(String[] args){
    System.out.println(Student.schoolName); // 传智播客
    Student.schoolName = "黑马程序员";
    System.out.println(Student.schoolName); // 黑马程序员
}
```

### 2.2.2 实例变量及其访问

无static修饰的成员变量属于每个对象的，  这个成员变量叫**实例变量**，之前我们写成员变量就是实例成员变量。

**需要注意的是**：实例成员变量属于每个对象，必须创建类的对象才可以访问。   

**格式：对象.实例成员变量**

### 2.2.3 静态方法及其访问

有static修饰成员方法，说明这个成员方法是属于类的，这个成员方法称为**类方法或者静态方法**。 直接用  类名访问即可。因为类只有一个，所以静态方法在内存区域中也只存在一份。所有的对象都可以共享这个方法。

与静态成员变量一样，静态方法也是直接通过**类名.方法名称**即可访问。



几个关键点：

* 静态方法只能访问静态（**可以从创建流程解释，静态要早于实例对象的创建，你会发现静态内容创建好时，实例都还没有呢怎么访问？**）
* 非静态方法能访问所有
* 静态方法中，没有this关键字（和上面原因一致）



**举例**

```java
public class Student{
    public static String schoolName = "传智播客"； // 属于类，只有一份。
    // .....
    public static void study(){
    	System.out.println("我们都在黑马程序员学习");   
    }
}
```

**静态成员变量的访问:**

**格式：类名.静态方法**

```java
public static void  main(String[] args){
    Student.study();
}
```

### 2.2.4 实例方法及其访问

无static修饰的成员方法属于每个对象的，这个成员方法也叫做**实例方法**。

**需要注意的是**：实例方法是属于每个对象，必须创建类的对象才可以访问。  

**格式：对象.实例方法**

**示例**：

```java
public class Student {
    // 实例变量
    private String name ;
    // 2.方法：行为
    // 无 static修饰，实例方法。属于每个对象，必须创建对象调用
    public void run(){
        System.out.println("学生可以跑步");
    }
	// 无 static修饰，实例方法
    public  void sleep(){
        System.out.println("学生睡觉");
    }
    public static void study(){
        
    }
}
```

```java
public static void main(String[] args){
    // 创建对象 
    Student stu = new Student ;
    stu.name = "徐干";
    // Student.sleep();// 报错，必须用对象访问。
    stu.sleep();
    stu.run();
}
```

## 2.3 小结

1.当 `static` 修饰成员变量或者成员方法时，该变量称为**静态变量**，该方法称为**静态方法**。该类的每个对象都**共享**同一个类的静态变量和静态方法。任何对象都可以更改该静态变量的值或者访问静态方法。但是不推荐这种方式去访问。因为静态变量或者静态方法直接通过类名访问即可，完全没有必要用对象去访问(静态方法中没有this)。

2.无static修饰的成员变量或者成员方法，称为**实例变量，实例方法**，实例变量和实例方法必须创建类的对象，然后通过对象来访问。

3.`static`修饰的成员属于类，会存储在静态区，是随着类的加载而加载的，且只加载一次，所以只有一份，节省内存。存储于一块固定的内存区域（静态区），所以，可以直接被类名调用。它优先于对象存在，所以，可以被所有对象共享(**所以有些常量就可以用static来修饰**)。

4.无static修饰的成员，是属于对象，对象有多少个，他们就会出现多少份。所以必须由对象调用。