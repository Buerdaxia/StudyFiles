# Ajax发送post请求

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <form>
    用户名:<input id="username" type="text" name="username"></input><br><br>
    密&emsp;码:<input id="password" type="password" name="password"></input><br><br>
    <input id="obtn" type="button" value="提交"></input>
    <div id="odiv"></div>
  </form>
  <script>
    let button = document.getElementById('obtn');
    let odiv = document.getElementById('odiv');
    button.onclick = function () {
      let username = document.getElementById('username').value;
      let password = document.getElementById('password').value;
      // 关键在于这里，传递数据的转换
      let data = {
        username,
        password
      }
      // 往后端传递数据时不能直接传递一个对象
      // 需要转换一下
      console.log(username, password);

      let xhr = new XMLHttpRequest();
      xhr.open('POST', '/login_post');
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
          // 成功之后的回调函数
          odiv.innerHTML = xhr.responseText;
        }
      }
      // 往后端传递数据时不能直接传递一个对象
      // 需要转换一下
      xhr.send(JSON.stringify(data));
    }
  </script>
</body>

</html>
```

## 传递数据

利用就是`JSON.stringify()`先将对象转换为json字符串，再进行传递，服务端接收到数据之后，再调用`JSON.parse()`转成对象就能直接操作了

