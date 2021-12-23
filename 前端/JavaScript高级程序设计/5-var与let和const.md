# var与let和const

## var 

var在es6发布之前，一直都是JavaScript声明变量的唯一手段。

var声明的变量，变量会被自动添加到最接近的函数执行上下文中。

```js
function add(num1, num2) { 
 var sum = num1 + num2; 
 return sum; 
} 
let result = add(10, 20); // 30 
console.log(sum); // 报错：sum 在这里不是有效变量
```

---

```js
function add(num1, num2) { 
 sum = num1 + num2; 
 return sum; 
} 
let result = add(10, 20); // 30 
console.log(sum); 
//这一次没有使用var关键字，然后sum被添加到全局上下文中
//注意：这种不使用关键字而初始化变量的操作是错误的
```



## let

let关键字的作用和var很像，但是是块级作用域的。

块级作用域由最近的一对包含花括号`{}`界定。

if 块、while 块、function 块，甚至连单独 的块也是 let 声明变量的作用域

并且，let与var另一个不同是，不能声明同名变量

```
var a;
var a;//不会报错

let b;
let b;//会报错 SyntaxError:
```

严格来讲，let 在 JavaScript 运行时中也会被提升，但由于“暂时性死区”（temporal dead zone）的 缘故，实际上不能在声明之前使用 let 变量。

## const

const关键字，使用时必须初始化，一经声明，在其生命周期的任何时候都不能再重新赋予新值。

除了要遵循以上规则意外，其余方面与let的声明规则一致。

s