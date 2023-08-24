# properties配置文件



概述：

properties是一个双列集合，**拥有Map集合所有的特点**（说明可以使用Map集合的方法）





要求：

1. 文件名以`.properties`结尾
2. 文件内容以键值对方式存在

```
name=钱不二
age=18
```



代码示例：

```java
Properties prop = new Properties();

// 虽然可以往properties中添加任意类型数据，但是默认规定添加String类型
prop.put("name", "钱不二");
prop.put("age", "18");
```







## properties特有的读写方法



| 方法                             | 解释                                             |
| -------------------------------- | ------------------------------------------------ |
| load(IO基本流)                   | 将properties文件内容以键值对方式加载进prop对象中 |
| store(IO基本流, String 注释信息) | 将prop对象中的内容写出到properties文件中         |
|                                  |                                                  |



load方法示例：

```java
Properties prop = new Properties();
FileInputStream fis = new FileInputStream("day35-code\\src\\test2\\prop.properties");
prop.load(fis); // 将配置文件信息读取到prop中
fis.close();
```



store方法示例：

```java
Properties prop = new Properties();
prop.put("name", "钱不二");
prop.put("age", "18");
prop.put("gender", "男");

FileOutStream = fos new FileOutputStream("day35-code\\src\\test2\\prop.properties");
prop.store(fos, "test");
fos.close();
```

写出的properties文件内容：

```
#test
#.....
name=钱不二
age=18
gender=男
```

