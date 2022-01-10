# Ajax发送get请求

```js
      // 发送ajax请求
      // 1.创建对象
      let xhr = new XMLHttpRequest();
      // 2.设置请求方式，路径
      // 路径和看后端写的啥
      xhr.open('GET', '/getData');

      // 3.绑定监听处理函数
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4 && xhr.status == 200) {
          odiv.innerHTML = xhr.responseText;
        }
      }

      // 4.发送请求
      xhr.send();
```

## get请求参数

get方式请求的参数一般跟在url后面，？开头 $分隔

```
xhr.open('GET', '/getData?username="xxx"&password="123456"');
```

