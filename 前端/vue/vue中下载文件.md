# vue中下载文件

## 一、原生的a标签下载

格式：`<a href="" download="" >`

href：要填写下载地址，例如：`/api/download`等等,最好写上域名，没有域名会自己添加项目的域名

download：一般不需要，但是用于规定作为文件名来使用的文本，最好还是带上以防止格式问题

注意: **一般这样下载文件的都是不带token等验证的下载方法,如果必须带token,可以和后端协商在href后面拼接,然后后端去接受验证**

## 二、通过按钮来来进行下载

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




