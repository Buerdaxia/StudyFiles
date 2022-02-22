# vue中下载文件

## 一、原生的a标签下载

格式：`<a href="" download="" >`

href：要填写下载地址，例如：`/api/download`等等,最好写上域名，没有域名会自己添加项目的域名

download：一般不需要，但是用于规定作为文件名来使用的文本，最好还是带上以防止格式问题

注意: **一般这样下载文件的都是不带token等验证的下载方法,如果必须带token,可以和后端协商在href后面拼接,然后后端去接受验证**

## 二、通过按钮来进行下载

这种不用担心`token`验证，应为`token`验证可以在路由守卫中验证或者`axios`的拦截器。

**这种方式最好还是将文件名传递过来，否则会有一些小问题**

原理：还是通过`a`标签来进行下载

```html
<el-button @click="downloadBackup(scope.row)" type="text"></el-button>
```

`点击函数`:

```js
async downloadBackup(row) {
  // 发起请求，获取数据
  const res = await this.$http({
    url:'xxx',
    method:'post'
  })
  
  const blob = new Blob([res.data]);
  const fileName = row.fileName;
  // 要创建出来一个a标签
  const link = document.createElement('a');
  link.download = fileName;
  link.style.display = 'none';// 隐藏a标签
  link.href = URL.createObjectURL(blob);
  document.body.appendChild(link);// 将a标签添加到页面中
  link.click();// 触发原生点击函数
  URL.revokeObjectURL(link.href);
  document.body.removeChild(link);// 移除这个a标签(名字叫link)
}
```

注意:**虽然不用担心token验证,但是一般通过接口返回的是数据,会默认在返回里面打开,只能看到完整的数据,但并不会变成下载,所以此处需要通过原生js创建一个a标签链接,设置隐藏属性以及默认点击,通过把数据转成Blob的方式,通过 URL.createObjectURL转化成href的格式被a标签识别,点击后再清除数据和a标签,谨防下次下载数据残留**





## Bolb对象

### 创建

可以通过Blob的构造函数创建Blob对象：

`new Blob(blobParts[, options])`

参数说明：

*blobParts*：**数组类型**，数组中的每一项连接起来构成的Blob对象的数据，数组中的每项元素可以是`ArrayBuffer(二进制数据缓冲区), ArrayBufferView,Blob,DOMString`。或其他类似对象的混合体。

*options*： 可选项，字典格式类型，可以指定如下两个属性：

- type，默认值为`""`，它代表了将会被放入到blob中的数组内容的MIME类型。
- endings， 默认值为"transparent"，用于指定包含行结束符`\n`的字符串如何被写入。 它是以下两个值中的一个： "native"，表示行结束符会被更改为适合宿主操作系统文件系统的换行符； "transparent"，表示会保持blob中保存的结束符不变。



```js
let data1 = ['a'];
let data2 = ['a', 'b'];
let data3 = ['abcdefg'];
let data4 = { name: '123', age: '18' };
console.log(new Blob(data1));
console.log(new Blob(data2));
console.log(new Blob(data3));
console.log(data4.toString());
console.log(new Blob([data4]));// 这里会先调用一下toString再计算size所以size时15个
/*
Blob {size: 1, type: ''}
Blob {size: 2, type: ''}
Blob {size: 7, type: ''}
[object Object]
Blob {size: 15, type: ''}
*/
```



## slice方法

Blob对象有一个`slice`方法，返回一个新的`Blob`对象,格式：

`Blob实例.slict(start, end)`

start： 可选，代表 [`Blob`](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 里的下标，表示第一个会被会被拷贝进新的 [`Blob`](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 的字节的起始位置。如果传入的是一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。

end： 可选，代表的是 [`Blob`](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 的一个下标，这个下标-1的对应的字节将会是被拷贝进新的[`Blob`](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 的最后一个字节。如果你传入了一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。

contentType： 可选，给新的 [`Blob`](https://link.juejin.im?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 赋予一个新的文档类型。这将会把它的 type 属性设为被传入的值。它的默认值是一个空的字符串。

示例：

```js
var data = "abcdef";
var blob1 = new Blob([data]);
var blob2 = blob1.slice(0,3);

console.log(blob1);  //输出：Blob {size: 6, type: ""}
console.log(blob2);  //输出：Blob {size: 3, type: ""}
```



使用Blob实现**分片上传**

```js
  reader.onload = function(e) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url);
    // 强制使用application/octet-stream类型
    xhr.overrideMimeType("application/octet-stream");
    
    xhr.onreadystatechange = function() {
      if(xhr.readyState === 4 && xhr.status ===200) {
        ++offset;
        if(offset === chunkQuantity) {
          alerrt("上传完成");
        } else if(offset === chunckQuantity-1) {
          blob = file.slice(offset*chunkSize, totalSize);
          reader.readAsBinaryString(blob);
        } else {
          blob = file.slice(offset*chunkSize, (offset+1)*chunckSize);
          reader.readAsBinaryString(blob);
        }
      }else {
        alert("上传出错");
      }
    }

    if(xhr.sendAsBinary) {
      xhr.sendAsBinary(e.target.result);
    } else {
      xhr.send(e.target.result);
    }
  }
  var blob = file.slice(0, chunkSize);
  reader.readAsBinaryString(blob);
}
```





### Bolb URL

Bolb URL是blob协议的URL，格式如下

`blob:http://xxxx`

Blob URL可以通过`URL.createObjectURL(blob)`创建。在绝大部分场景下，可以像使用`http`一样来使用Blob URL。

常见的场景有：作为文件的下载地址（如上面的通过按钮来进行下载）或者作为图片的资源路径





作为图片的资源路径

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Blob Test</title>
  <script>
    function handleFile(e) {
      var file = e.files[0];
      var blob = URL.createObjectURL(file);
      var img = document.getElementByTagName("img")[0];
      img.src = blob;
      img.onload = function(e) {
        URL.revokeObjectURL(this.src); //释放createObjectURL创建得对象
      }
    }
  </script>
</head>

<body>
  <input type="flie" accept="image/*" onchange="handleFile(this)" />
  <br/>
  <img style="width:200px;height:200px;">
</body>

</html>
```



### window.URL.revokeObjectURL()

在每次调用`URL.createObjectURL()`方法时，都会创建一个新的URL对象。

当不在需要这些URL对象时，每个对象必须通过调用`URL.revokeObjectURL()`的方法来释放。

浏览器虽然会在文档退出时自动释放，但是为了获得最佳性能和内存使用状况，应当在安全的时机主动释放它们。



## 那么Blob URL和Data URL有什么区别呢？

1. Blob URL得长度一般比较短，但Data URL因为直接存储图片base64编码后得数据，往往很长。当显示大图片时，使用Blob URL更优。
2. Blob URL可以方便的使用XMLHttpRequest获取源数据，例如：



```jsx
var blobUrl = URL.createObkectURL(new Blob(['Test'], {type: 'text/plain'}));
var xhr = new XMLHttpRequest();
//如果是指xhr.responseType = 'blob',将返回一个Blob对象，而不是文本；
//xhr.responseType = 'blob';
xhr.onload = function() {
  alert(xhr.responseText);
}
xhr.open('get', blobUrl);
xhr.send();
```

对于Data URL， 并不是所有浏览器都支持通过XMLHttpRequest获取源数据的。

1. **Blob URL只能在当前应用内部使用**，把Blob URL复制到浏览器的地址栏中，是无法获取数据的。Data URL相比之下，就有很好的移植性，你可以在任意浏览器使用。

---

除了可以用作图片资源的网络地址，Blob URL也可以用作其他资源的网络地址，例如html文件、json文件等，为了保证浏览器能正确的解析Blob URL返回的文件类型，需要在创建Blob对象时指定相应的type：

```js
//创建HTML文件的Blob URL
var data = "<div style='color:red;'This is a blob</div>";
var blob = new Blob([data], {type: 'text/html'}); // 'application/json'
var blobUrl = URL.createObjectURL(blob);
```





Blob部分作者及链接如下

作者：cctosuper
链接：https://www.jianshu.com/p/b322c2d5d778
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
