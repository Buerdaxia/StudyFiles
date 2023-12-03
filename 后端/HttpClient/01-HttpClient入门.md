# HttpClient入门



## 介绍

HttpClient时Apache Jakarta Common下的子项目，可以用来提供高效的、最新的、功能吩咐的支持HTTP协议的**客户端编程工具包**，并且它支持HTTP协议最新的版本和建议。

简单来说：**可以在Java程序中通过HttpClient工具包能够通过编码的方式构造并发送HTTP请求**





## 核心API



| API                | 作用               | 属性                       |
| ------------------ | ------------------ | -------------------------- |
| HttpClient         | 发送Http请求       | 接口                       |
| HttpClients        | 创建HttpClient对象 | 类                         |
| CloseabeHttpClient | 一个实现类         | 实现类(实现HttpClient接口) |
| HttpGet            | HTTP GET请求       | 类                         |
| HttpPost           | HTTP POST请求      | 类                         |



## 使用步骤

### 引入坐标

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
```

>注意：
>
>如果我们项目中也用了阿里云的OSS其实就不用引入了，阿里云oss的SDK中引入了，当然我们自己再引



步骤：

1. 引入坐标
2. 创建HttpClient对象
3. 创建Http请求对象
4. 调用HttpClient的execute方法发送请求



### GET请求

GET请求具体示例：

```java
/**
 * 通过httpclient发送get请求
 */
@Test
public void testGET() throws Exception{
    // 创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();

    // 创建请求对象
    HttpGet httpGet = new HttpGet("http://localhost:9090/user/shop/status");

    // 发送请求并接受响应结果
    CloseableHttpResponse response = httpClient.execute(httpGet);

    // 获取服务端返回的状态码
    int statusCode = response.getStatusLine().getStatusCode();
    System.out.println("服务端返回的状态码：" + statusCode);

    // 响应结果具体内容
    HttpEntity entity = response.getEntity();
    String body = EntityUtils.toString(entity); // 要解析一下
    System.out.println("服务端返回的数据为：" + body);

    // 关闭资源
    response.close();
    httpClient.close();
}
```



### POST请求

POST请求具体示例：

```java
/**
 * 通过httpclient发送post请求
 */
@Test
public void testPOST() throws Exception{
    // 创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();

    // 创建请求对象
    HttpPost httpPost = new HttpPost("http://localhost:9090/admin/employee/login");


    // 构造请求体
    JSONObject jsonObject = new JSONObject(); // 构造json
    jsonObject.put("username", "admin");
    jsonObject.put("password", "123456");

    StringEntity entity = new StringEntity(jsonObject.toString()); // 要传入一个json字符串
    // 指定请求编码方式
    entity.setContentEncoding("utf-8"); // 编码方式
    entity.setContentType("application/json"); // 内容格式
    httpPost.setEntity(entity);

    // 发送请求
    CloseableHttpResponse response = httpClient.execute(httpPost);

    // 解析返回结果
    int statusCode = response.getStatusLine().getStatusCode();
    System.out.println("响应码：" + statusCode);

    HttpEntity entity1 = response.getEntity();
    String body = EntityUtils.toString(entity1);
    System.out.println("响应体内容：" + body);

    // 关闭资源
    response.close();
    httpClient.close();
}
```





### 封装工具类

正常使用我们都会自己简单封装一下，下面收集的封装代码来自黑马教程。



`utils/HttpClientUtil.class`

```java
package com.sky.utils;

import com.alibaba.fastjson.JSONObject;
import org.apache.http.NameValuePair;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/**
 * Http工具类
 */
public class HttpClientUtil {

    static final  int TIMEOUT_MSEC = 5 * 1000;

    /**
     * 发送GET方式请求
     * @param url
     * @param paramMap
     * @return
     */
    public static String doGet(String url,Map<String,String> paramMap){
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();

        String result = "";
        CloseableHttpResponse response = null;
		
        // 这里构造了一下有请求的URI（就是拼接了一下query参数）
        try{
            URIBuilder builder = new URIBuilder(url);
            if(paramMap != null){
                for (String key : paramMap.keySet()) {
                    builder.addParameter(key,paramMap.get(key));
                }
            }
            URI uri = builder.build();

            //创建GET请求
            HttpGet httpGet = new HttpGet(uri);

            //发送请求
            response = httpClient.execute(httpGet);

            //判断响应状态
            if(response.getStatusLine().getStatusCode() == 200){
                result = EntityUtils.toString(response.getEntity(),"UTF-8");
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                response.close();
                httpClient.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return result;
    }

    /**
     * 发送POST方式请求
     * @param url
     * @param paramMap
     * @return
     * @throws IOException
     */
    public static String doPost(String url, Map<String, String> paramMap) throws IOException {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";

        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);

            // 创建参数列表
            if (paramMap != null) {
                List<NameValuePair> paramList = new ArrayList();
                for (Map.Entry<String, String> param : paramMap.entrySet()) {
                    paramList.add(new BasicNameValuePair(param.getKey(), param.getValue()));
                }
                // 模拟表单
                UrlEncodedFormEntity entity = new UrlEncodedFormEntity(paramList);
                httpPost.setEntity(entity);
            }

            httpPost.setConfig(builderRequestConfig());

            // 执行http请求
            response = httpClient.execute(httpPost);

            resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
        } catch (Exception e) {
            throw e;
        } finally {
            try {
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }

    /**
     * 发送POST方式请求
     * @param url
     * @param paramMap
     * @return
     * @throws IOException
     */
    public static String doPost4Json(String url, Map<String, String> paramMap) throws IOException {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";

        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);

            if (paramMap != null) {
                //构造json格式数据
                JSONObject jsonObject = new JSONObject();
                for (Map.Entry<String, String> param : paramMap.entrySet()) {
                    jsonObject.put(param.getKey(),param.getValue());
                }
                StringEntity entity = new StringEntity(jsonObject.toString(),"utf-8");
                //设置请求编码
                entity.setContentEncoding("utf-8");
                //设置数据类型
                entity.setContentType("application/json");
                httpPost.setEntity(entity);
            }

            httpPost.setConfig(builderRequestConfig());

            // 执行http请求
            response = httpClient.execute(httpPost);

            resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
        } catch (Exception e) {
            throw e;
        } finally {
            try {
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }
    private static RequestConfig builderRequestConfig() {
        return RequestConfig.custom()
                .setConnectTimeout(TIMEOUT_MSEC)
                .setConnectionRequestTimeout(TIMEOUT_MSEC)
                .setSocketTimeout(TIMEOUT_MSEC).build();
    }

}

```

