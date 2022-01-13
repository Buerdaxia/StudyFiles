# Global对象身上一些重要的方法

ECMA-262 规定 Global 对象为一种兜底对象，它所针对的是不属于任何对象的属性和方法。在全局作用域中定义的变量和函数都会变成 Global 对象的属性 。例如：

`isNan()`,`parseInt()`,`parseFloat()`,`isFinite()`等方法实际上都是Global对象上的方法。以下还有几个重要的方法。



## URL编码方法

### encodeURI()和 encodeURIComponent()方法

encodeURI()和 encodeURIComponent()方法用于编码统一资源标识符（URI），以便传给浏览器。 有效的 URI 不能包含某些字符，比如空格。

语法:

```js
encodeURI('url')
encodeURIComponent('url')

// url就是资源标识符例如：www.xxx.com/sss.html
```

两个方法的主要区别是:

encodeURI()不会编码属于 URL 组件的特殊字符，比如冒号、斜杠、问号、 井号。

encodeURIComponent()会编码它发现的所有非标准字符。



代码示例：

encodeURI()：

```js
let uri = 'http://www.baidu.com/illegal value.js#start';
console.log(encodeURI(url));
// http://www.baidu.com/illegal%20value.js#start
// 会将空格转换为%20 其他字符没有变化
```

encodeURIComponent()：

```js
let url = 'http://www.baidu.com/illegal value.js#start';
console.log(encodeURIComponent(url));
//http%3A%2F%2Fwww.baidu.com%2Fillegal%20value.js%23start
// 会将所有非字母符号都替换成了相应的编码形式
```

### decodeURI()和 decodeURIComponent()

decodeURI()和 decodeURIComponent()是与上面方法相对应的两个方法

**注意：decodeURI()只对使用 encodeURI()编码过的字符解码。**

语法

```js
decodeURI('encodeURI()转换过的URL')

decodeURIComponent('转换过的URL')
```



代码示例：

decodeURl('encodeURI()转换过的URL')

```js
let url = 'http://www.baidu.com/illegal value.js#start';
//  http://www.baidu.com/illegal%20value.js#start
let enUrl = encodeURI(url);

console.log('decodeUR:', decodeURI(enUrl));
// decodeUR: http://www.baidu.com/illegal value.js#start
```

decodeURIComponent('转换过的URL'):

```js
let uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start";

console.log(decodeURIComponent(uri));
// http://www.wrox.com/illegal value.js#start
```



这两个方法的区别：

decodeURI()只能处理被encodeURI()编码过的字符。其余的字符编码不会解码。

而decodeURIComponent()方法会解码所有的特殊字符