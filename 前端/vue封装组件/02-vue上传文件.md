# vue上传文件

上传文件一般使用的的是`<input></input>`标签

上传接口：

```js
data: {
  file: xxxx
}
```





通过表单上传文件：

```html
<form action="http://localhost:8000/goods/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file">
  <!--这里name=file 就是上传时的data中的file参数名 -->
  <input id="sub" type="submit" value="上传">
  </form>
```



## vue中的文件上传

```vue
<template>
	<div>
    <input type="file" @change="getFile($event)" multiple="multiplt" />
    <button @click='submitAddFile'>
		上传
  </button>
  </div>
</template>

<script>
getFilt(event) {
  // 获取的是上传文件数组 关键就在这里
  let file = event.target.files;
  this.fileArr = file;
}
  
submitAddFile() {
  let formData = new FormData();
  this.fileArr.forEach((item) => {
    formData.append('file', item)
  })
  /*
  	将formData作为body进行发送
  */
  // 发送axios请求
  
}

</script>
```

