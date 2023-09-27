# fastjson

作用：一款快速转换/解析JSON的阿里巴巴的小工具

详细使用方式，请结合官方文档(●'◡'●)



## 使用

一：pom中引入

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>2.0.32</version>
</dependency>
```





例如：

`JSONObject.toJSONString(Object object)`方法：

作用：将一个对象转换成JSON字符串

```java
// 例如将字符串转换为JSON
String str = "钱不二哦";
String JsonStr = JSONObject.toJSONString(str); // 会成为一个json字符串
```





