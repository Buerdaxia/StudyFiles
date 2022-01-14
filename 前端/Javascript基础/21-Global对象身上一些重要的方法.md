# Global对象身上一些重要的方法

ECMA-262 规定 Global 对象为一种兜底对象，它所针对的是不属于任何对象的属性和方法。在全局作用域中定义的变量和函数都会变成 Global 对象的属性 。例如：

`isNan()`,`parseInt()`,`parseFloat()`,`isFinite()`等方法实际上都是Global对象上的方法。以下还有几个重要的方法。



**虽然没有规定直接访问Global的方式，但是它有代理人，那就是window对象**

## 一种简单获取Global对象的方式

```js
let global = function() {
  return this;
}()
```





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



## eval()方法

这个方法就是一个完 整的 ECMAScript 解释器，它接收一个参数，即一个要执行的 ECMAScript（JavaScript）字符串。

语法：

```
eval("语句");
```

当解释器发现`eval()`函数时，会将参数解释为实际的JavaScript语句并插入到该位置。

**通过 eval()执行的代码属于该调用所在上下文，被执行的代码与该上下文拥有相同的作用域链。这意 味着定义在包含上下文中的变量可以在 eval()调用内部被引用**

代码实例：

```js
let msg = "hello world"; 
eval("console.log(msg)"); // "hello world" 

//这里的’console.log('msg')‘会直接解析为console语句
```



也可以写成一个函数：

```js
eval('function sayHi() { console.log('h1')} ');
sayHi();
```

注意：**通过eval()定义的任何变量和函数都不会被提升**，因为在解析代码时，它们是被包裹在一个字符串中的。它只有在`eval()`执行的时候才会被创建。

严格模式下，`eval()`函数内部变量无法被访问。会报错