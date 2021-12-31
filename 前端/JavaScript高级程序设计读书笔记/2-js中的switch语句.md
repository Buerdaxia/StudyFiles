# js中的switch语句

语法：

```js
switch(expression) {
  case value1:
  statement
  break;
  
  case value2:
  statement
  break;
  
  ...
  
  default:
  statement
}
```



**特殊点就在于**：switch 语句可以用于所有数据类型（在很多语言中，它只能用于数值），因此可以使用字符串甚至对象。 其次，条件的值不需要是常量，也可以是变量或表达式

```js
switch ("hello world") { 
 case "hello" + " world": //这里可以是表达式
 console.log("Greeting was found."); 
 break; 
 case "goodbye": 
 console.log("Closing was found."); 
 break; 
 default: 
 console.log("Unexpected message was found."); 
} 
```



注意： **switch 语句在比较每个条件的值时会使用全等操作符（===），因此不会强制转换数据类 型（比如，字符串"10"不等于数值 10）。**