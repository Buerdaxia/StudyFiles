# Mockjs

## 什么是mockjs？

前端拜托后端接口束缚，前端工程师自己生成接口并生成随机数据，拦截Ajax。



## 为什么要用mockjs？

主要目的：提高前端人员开发效率，可以不必等待后端人员将后端接口建立完毕。手动来模拟接口数据给我们使用。



## 使用mockjs

>第一步：安装

```js
npm install mockjs
```

>第二步：在项目中要创建一个mock文件夹下面创建index.js

```js
// 文件的内容首先要引入mock模块
import Mock from 'mockjs';
```

>第三步：将mock.js文件在main.js中导入

```js
import Vue from 'vue';
import App from './App.vue';
import './mock/index.js';

...
```



## mock基本语法

基础语法：以一种键值对的方式来描述



### 生成字符串：

```js
Mock.mock({
  // 注意 前面键名不能有空格，否则报错
  "string|1-10": "★"
})
```

"string|1-10"解释：

前面是类型，后面是生成数量：1-10指随机生成

```js
Mock.mock({
  "string|1": "★" // 生成1个五角星
})
```



### 生成随机文本

函数中的：`c`指的是`chinese`中文

@cword(参数一,参数二)函数

作用：可以随机生成一串字符

参数一（单独使用）：可以指定生成字符数量

参数一，参数二（一起使用）：可以指定一个字符的范围

```js
const data = Mock.mock({
	string: '@cword(3,5)',
	str: '@cword(1)'
});

```





### 生成标题和句子

函数语法和上面生成随机文本一致：

```js
const data = Mock.mock({
	title: '@ctitle(5)',
	sentence: '@csentence(10)'
});
```

>指定范围：

```js
const data = Mock.mock({
	title: '@ctitle(5,10)',// 标题为5-10个随机字符
	sentence: '@csentence(10,20)'// 句子为10-20个随机字符
});
```



### 生成段落

函数语法与上面一致

```js
const data = Mock.mock({
	content: '@cparagraph(5)'
});
```



### 生成数字

* 生成指定数字

```js
const data = Mock.mock({
	number: 10
});
```

* 生成范围的数字

```js
const data = Mock.mock({
	'number|1-10': 10
});

```



### 生成增量id

* 随机生成标识

```js
const data = Mock.mock({
	id: '@increment()',
	id2: '@increment()'//自动加1
});
```



### 生成姓名-地址-身份证号

```js
const data = Mock.mock({
	name: '@cname()',
	id: '@id()',
	address: '@city(true)' // 有true包括省， 没true是单独的市
});
//{name: '易霞', id: '36000019750829418X', address: '上海 上海市'}
```



### 随机生成图片

函数：`@image(参数一，参数二，参数三，参数四，参数五)`

* 参数一：图片的大小

  ```js
  [// 自己规定
   	'300x300', '250x250'...
  ]
  ```

* 参数二：图片的背景色

* 参数三：图片的前景色

* 参数四：图片格式

* 参数五：图片文字

```js
const data = Mock.mock({
	img_url: "@image('250x250', '#ffa07a', '@ffbbff', 'png', '坤坤')"
});
```



### 生成时间

函数：`@date(参数一)`

作用：生成日期

参数一：可以将日期进行格式化，例如`yyyy-MM-dd hh:mm:ss`

```js
const data = Mock.mock({
	date: '@date(yyyy-MM-dd hh:mm:ss)'
});
// {date: '1975-05-22 00:22:19'}
```



### 指定数组返回的条数（arraylist）

* 随机生成5-10条数据

```js
const data = Mock.mock({
	'list|5-10': [ 
		{
			name: '@cname()',
			address: '@city(true)',
			id: '@increment()'
		}
	]
});
```



## mock拦截请求（重要）

定义完毕后，就可以直接请求接口

### 定义拦截get请求



```js
// 拦截get请求
Mock.mock('/api/news', 'get', (options)=>{
  return {
		statue:200,
    msg:'获取成功',
    list: [...]
 	}
});

// 参数一：url 可以写成正则
// 参数二：请求方式 注意一定是小写get，GET就拦截不到了
// 参数三：返回数据，
```



### 定义拦截post请求

```js
Mock.mock('/api/post/news', 'post', (options) => {
  return {
		statue:200,
    msg:'获取成功',
    list: [...]
 	}
});
// 参数一：url
// 参数二：请求方式 注意一定是小写post，post就拦截不到了
// 参数三：返回数据，一般是一个函数，可以做很多事情
```





## 整体分装（真正的使用(ง •_•)ง）

### get请求

```js
const { newsList } = Mock.mock({
	'newsList|55': [
		{
			id: '@increment()',
			title: '@ctitle()',
			content: '@cparagraph()',
			img_url: "@image('100x100', '#FF83FA', '#fcfcfc', 'png', '123')",
			add_time: '@date(yyyy-MM-dd)'
		}
	]
});
/**
 *
 * @param {string} url
 * @param {string} name
 * @description 获取url中的指定参数 后续可以将这个方法抽离出来
 */
const getQuery = (url, name) => {
	const index = url.indexOf('?');
	if (index !== -1) {
		const queryArr = url.substr(index + 1).split('&');
		for (let i = 0; i < queryArr.length; i++) {
			const itemArr = queryArr[i].split('=');
			if (itemArr[0] === name) {
				return itemArr[1];
			}
		}
	} else {
		// 如果没有问号
		return null;
	}
};

// 接口名写成正则的原因：get请求携带参数后会修改接口名 
Mock.mock(/\/api\/get\/news/, 'get', options => {
	const pageIndex = getQuery(options.url, 'pageIndex');
	const pageSize = getQuery(options.url, 'pageSize');
	console.log(pageIndex, pageSize);

	// 计算总页数
	const totalPage = Math.ceil(newsList.length / pageSize);

	// 我们要根据pageIndex,和pageSize来截取数据进行传输
	const start = (pageIndex - 1) * 10;
	const end = pageIndex * 10;
	// 判断 传入参数是否大于总页数，大于返回空数组
	const list = pageIndex > totalPage ? [] : newsList.slice(start, end);
	return {
		status: 200,
		msg: '获取成功',
		list: list,
		total: list.length
	};
});
```



### post请求

```js
// 定义添加新闻列表的数据
Mock.mock('/api/add/news', 'post', options => {
	// post拿参数比较方便，直接转换
	const body = JSON.parse(options.body);
	console.log(body);

	// 添加数据，直接push到list中
	newsList.push(
		Mock.mock({
			id: '@increment()',
			title: body.title,
			content: body.content,
			img_url: "@image('100x100', '#FF83FA', '#fcfcfc', 'png', '123')",
			add_time: '@date(yyyy-MM-dd)'
		})
	);
	return {
		status: 200,
		msg: '获取成功',
		list: newsList,
		total: newsList.length
	};
});
```





### 接口文档

>获取新闻

接口地址：`/api/get/news`

接口参数：

```js
pageIndex: 1
pageSize: 10
```

请求类型：`GET`

返回的数据： 

```js
{
	status: 200,
  message:'添加成功',
  list: [
    {
        id: '1',
  			title: '标题',
  			content: '内容',
  			img_url: "xxxxx",
  			add_time: '1999-04-03 11:43:37'
    }
  ],
	total: 50
}
```







> 添加新闻

接口地址：`/api/add/news`

接口参数：

```js
title: 标题
content: 内容
```

请求类型：`POST`

返回的数据：

```js
{
	status: 200,
  message:'添加成功',
  list: [
    {
        id: '1',
  			title: '标题',
  			content: '内容',
  			img_url: "xxxxx",
  			add_time: '1999-04-03 11:43:37'
    }
  ],
	total: 50
}
```



