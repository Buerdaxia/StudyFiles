# el-upload组件

首先看官网el-upload组件的介绍

![el-upload组件](../../前端图片/ElementUI/el-upload/el-upload组件.png)

注意:在data中绑定的这个fileList，暂时访问不到数据，就是在以下的阶段，如果你初始化为`fileList: []`则之后的阶段一直是空的。



如果你想获取你上传的文件，最好通过

```
this.$refs.upload.uploadFiles
//该属性可以访问到你选择文件之后添加到列表里的文件
```



例如：就可以利用这个属性来判断一些空值，例如没有上传文件时，点击上传会报未定义错误

```js
//解决方式，提交前先判断下有没有文件
      if (this.$refs.upload.uploadFiles.length !== 0) {
        // 点击提交上传文件
        this.$refs.upload.submit();
      } else {
        this.$message.error('请选择文件');
      }
```





## 上传时对文件类型进行判断

可以在upload自带的`beforeUpload`钩子中进行类型判断。该钩子会传递一个`file参数`里面有上传文件的信息

![before-upload](../../前端图片/ElementUI/el-upload/before-upload.png)

例如：

```js
    // 限制选择文件类型
    beforeUpload(file) {
      // 找到.的位置下标
      const start = file.name.indexOf('.') + 1;
      // 截取文件后缀名
      const fileType = file.name.slice(start);
      const type = ['csv', 'xls', 'xlxs'];
      // 判断上传文件类型
      if (!type.includes(fileType)) {
        this.$message.error('只能上传csv,xls,xlxs文件');
      }
      // 返回false则停止上传
      return type.includes(fileType);
    }
```



## 清除列表

清除列表可以使用`upload`组件自带的函数`clearFiles`进行清除



## 获取上传的文件

原始的获取上传文件方法：(VUE中)

html部分：

```html
<inpt type="file" @change="uploadConfig">
<!-- type要改成file表示要上传文件 上传文件还有一些参数可以限制上传类型-->
```

js部分：

```js
uploadConfig(e) {
    // 整个文件会放在这个e.target.files这个数组中
	console.log(e.target.files)
    // 获取第一个文件就e.target.file[0]
}
```







上传的文件信息放在`this.$refs.upload.uploadFiles`

![upload上传文件](../../前端图片/ElementUI/el-upload/upload上传文件.PNG)

注意：**其中raw属性中放着文件**

我们获取这个文件就可以

```
this.$refs.upload.uploadFiles[0].raw
// 获取这个上传文件
```

