# URL编码问题

## encodeURI和decodeURI

这两个方法一个用来编码`URL`，一个用来解码`URL`。

作用：**对整个URL进行编码和解码，用来处理URL中出现的中文和空格**

代码：

```js
    let url = "http://www.baidu.com? lx=百度&id=10";
    let encodeU = encodeURI(url);
    let decodeU = decodeURI(encodeU);
    console.log(encodeU);// http://www.baidu.com?%20lx=%E7%99%BE%E5%BA%A6&id=10
    console.log(decodeU); // http://www.baidu.com? lx=百度&id=10
```



## encodeURIComponen和decodeURIComponent

也是进行编码和解码的，但是区别是这个是对部分`URL`进行编码

作用：主要对传递的参数信息进行编码，不仅可以对中文进行编码，还可以对一些特殊字符进行编码，例如，/等

代码：

```js
let url2 = `http://www.qianbuer.com?lx=${encodeURIComponent('钱不二')}&from=${encodeURIComponent('http://www.badu.com')}`;
    console.log(url2);
// http://www.qianbuer.com?lx=%E9%92%B1%E4%B8%8D%E4%BA%8C&from=http%3A%2F%2Fwww.badu.com
```

