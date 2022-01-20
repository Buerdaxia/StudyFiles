# art-templates模板传递数据

首先应当先获取数据,这里写的假数据

```js
app.get('/', (req, res) => {
	// 我们可以再接口中查询数据库，得到数据后传递给模板，展示在页面上
	let data = {
		num1: 31,
		num2: 30,
		user: {
			name: '钱不二',
			age: 18
		},
		books: ['三国演义', '水浒传', '红楼梦', '西游记']
	};

	// 这样可以将data传递到index中
	res.render('index', data);
});
```



然后利用模板引擎自带的插值语法，可以读取传递的数据并展示到页面上。

`index.html`中的语句：

```html
  <!-- 获取data中普通的键值对儿 -->
  <p>num1是：{{num1}}</p>
  <p>num2是：{{num2}}</p>
  <hr>
  <!-- 获取data中的对象值 -->
  <p>{{user.name}}</p>
  <p>{{user.age}}</p>

  <hr>
	<!-- 循环-->
  <ul>
    {{each books}}
    <!-- books 是数组名， $index是索引值，$value是数组元素值 -->
    <li>{{ $index }}、{{ $value }}</li>
    {{/each}}
  </ul>

  <hr>
  <!-- 如果num1 > num2 就显示下面的p标签 -->
  {{if num1 > num2 }}
  <p>这个标签根据条件显示</p>
  {{/if}}
```

插值语句官网中有介绍。直接去template官网

