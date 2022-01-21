# pathinfo参数的使用

第一步：怎么传递？

直接将请求参数内容，写在请求路径后面

```html
<body>
  <h1>List页面</h1>
  <ul>
    <li><a href="/detail/1/music">新闻标题1</a></li>
    <li><a href="/detail/2/sport">新闻标题2</a></li>
    <li><a href="/detail/3/food">新闻标题3</a></li>
    <li><a href="/detail/4/car">新闻标题4</a></li>
  </ul>
</body>
```



第二步：怎么接收？

在处理请求时的路径后面跟上`:id`，`:type`等动态参数接受

```js

// 在路径后写入/:id表示id是动态参数
app.get('/detail/:id/:type', (req, res) => {
});
```



第三步：怎么获取？

通过`req.params`获取动态参数，是一个对象，属性名是上面动态参数名

```js
// 在路径后写入/:id表示id是动态参数
app.get('/detail/:id/:type', (req, res) => {
	console.log(req.params);
	console.log(req.params.id);
	console.log(req.params.type);
	res.send('detail详情页');
});

```

