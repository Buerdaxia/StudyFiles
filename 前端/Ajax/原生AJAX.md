# 原生AJAX

AJAX全称Asynchronous JavaScript And XML，就是异步的JS和XML。

**通过AJAX可以再浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。**

异步请求，局部刷新

AJAX不是新的变成语言，而是一种将现有的标准组合在一起的使用方式。

# XML(曾经是AJAX使用的传输方式)

XML叫可扩展标记语言。

XML被设计用来传输和储存数据。

XML和HTML类似，但是HTML中都是预定义标签，而XML没有预定义标签，全部都是自定义标签，用来表示一些数据。

```xml
比如有一个学生数据：
	name = "钱不二";age = 18; gender = "男";
用XML表示：
	<student>
		<name>钱不二</name>
        <age>18</age>
        <gender>男</gender>
	</student>
```

# Ajax优缺点

优点：

1. 无需刷新页面而与服务器端进行通信。
2. 允许你根据用户事件来更新部分页面内容。

缺点：

1. 没有浏览历史，不能回退
2. 存在跨域问题(同源)
3. SEO不友好

