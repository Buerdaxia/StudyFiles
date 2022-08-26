# jQuery中$符号的冲突问题

## jQuery中$符号的冲突问题

问题：当引入的其他代码或者框架中，也使用了$符，那么就会影响到jQuery的编译(应为jq也使用$来简写代码)。

解决方式一：释放使用权

```js
// 1.释放$的使用权
// 注意点:释放操作必须在编写其他代码之前写,释放之后不能再使用$
jQuery.noConflict();//释放$使用权

// 之后将所有$符号替换为jQuery
jQuery(function() {
alert('hello jq');
});
```

解决方式二：自定义

```js
// 2.自定义访问符号 
var qec = jQuery.noConflict();

// 之后qec就替换了之前的$
qec(function() {
alert('hello jq');
});
```

