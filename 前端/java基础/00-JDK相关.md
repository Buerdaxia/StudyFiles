# JDK相关

## 下载

地址：`https://www.oracle.com/java/technologies/downloads`

## 验证JDK安装成功

命令行输入：`java -version`



## Javac和java

javac是Java程序的编译器，用来编译java文件的。



javac 编译命令

java 执行命令



## JDK组成

* JVM（Java Virtual Machine）：Java虚拟机，真正运行Java程序的地方
* 核心类库：Java自带的程序，给程序员使用(调用)
* JRE（Java Runtime Environment）：Java的运行环境
* JDK（Java Development kit） ：Java开发工具包（包括以上所有）例如javac、java



## 跨平台

Java的跨平台：一次编译，处处可用。

原因：

是应为现在有不同平台（Linux、Windows、macOS）都有对应平台版本的JVM，只要有JVM的地方，就能够运行javac编译出来的.class文件。