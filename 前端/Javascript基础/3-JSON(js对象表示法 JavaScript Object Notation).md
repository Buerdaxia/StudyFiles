# JSON(js对象表示法 JavaScript Object Notation)

！！！JS中的对象只有JS自己认识，其他语言都不认。

**JSON就是一个特殊格式的字符串，这个字符串可以被任意的语言所识别，并且可以转换为任意语言中的对象。**

所以！JSON在开发中主要就用来数据的交换。

格式：JSON和js对象一样，不过JSON字符串中的属性名必须加双引号，其他的和js语法一直。

js中的对象：var obj = {name: "钱不二"};**(name引号可加可不加)**

json字符串：var obj = '{"name": "钱不二"}';**(name必须加引号)**

## JSON 中允许的值

1. 字符串
2. 数值
3. 布尔值
4. null
5. 对象(就只是普通的对象 **像函数那种对象不行**)
6. 数组

## JSON工具类

在js中提供了一个工具类就叫JSON,这个工具类可以将对象转换为json也可以将json转换为对象。

### JSON.parse() 将一个json字符串转换为js对象

语法：JSON.parse(参数1) 

参数1：是一个JSON字符串



注意：JSON字符串也可能是一个空串例如

```js
JSON.parse('""')
//传递的就是一个空串
```



### JSON.sringify(参数1)将一个js对象转换为json字符串

语法：`JSON.stringify(内容);`



## 判断一个字符串是否能转换

```js
function transform(str) {
  try{
    if(typeof JSON.parse(str) == 'object') {
      return true;
    }
  }catch() {
  }
  return false;
}
```



