# buffer缓冲器

JavaScript语言本身只有字符串数据类型，并没有二进制数据类型。但是在处理文件流时（文件读写操作），必须使用到二进制数据。因此在Node.js中定义了一个Buffer类，该类用来创建一个专门存放二进制数据的缓存区。说白了Buffer类似于一个整数数组。

`Buffer` 对象用于表示固定长度的字节序列。**不能进行修改大小**

## buffer的常用方法

### `Buffer.alloc(size[, fill[, encoding]])`



