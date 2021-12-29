# buffer缓冲器

JavaScript语言本身只有字符串数据类型，并没有二进制数据类型。但是在处理文件流时（文件读写操作），必须使用到二进制数据。因此在Node.js中定义了一个Buffer类，该类用来创建一个专门存放二进制数据的缓存区。说白了Buffer类似于一个整数数组。

`Buffer` 对象用于表示固定长度的字节序列。**不能进行修改大小**

```js
let buf1 = Buffer.from([97, 98, 99]);
console.log(buf1); // <Buffer 61 62 63>是一个16进制的数据
console.log(buf1.toString()); // abc
```

## 关键（toString()方法）

当看到一个`<Buffer xx xx xx xx >`就知道，这是一个`Buffer`对象，调用一下`toString()`方法即可转换为能阅读的信息

```js
let buf2 = Buffer.from('node.js');
console.log(buf2); //<Buffer 6e 6f 64 65 2e 6a 73>
console.log(buf2.toString());//node.js
```

## buffer的常用方法(后续更新)

### Buffer.alloc(xx)

创建一个

