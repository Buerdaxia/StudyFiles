# 原始包装类Nuber的方法

Number 是对应数值的引用类型。要创建一个 Number 对象，就使用 Number 构造函数并传入一个 数值，如下例所示：

```js
let numberObject = new Number(10);
```



## toString(参数)方法

该方法接受一个参数作为标识基数的参数，并返回相应基数形式的数值字符串。

```js
let num = 10;
// 返回的是字符串
console.log(num.toString()); // "10"  默认是10进制
console.log(num.toString(2)); // "1010" 
console.log(num.toString(8)); // "12" 
console.log(num.toString(10)); // "10" 
console.log(num.toString(16)); // "a"
```



## toFixed(参数)方法

`toFixed()`方法，返回包含指定小数点位数的数值字符串。

```js
let num = 10;
console.log(num.toFixed());//  默认是"10"，没有小数
console.log(num.toFixed(2));//  "10.00" 保留2位小数
```

如果数值位数，多余指定的参数，则会采取四舍五入方式到最近的小数位。

```js
let num = 10.005;
console.log(num.toFixed(2));//  "10.01"四舍五入到2位
```

## toPrecision(参数)方法

toPrecision()方法会根据情况返回最合理的输出结果，可能是固定长度，也可能是科学记数法 形式。这个方法接收一个参数，表示结果中数字的总位数（不包含指数）。

```js
let num = 99; 
console.log(num.toPrecision(1)); // "1e+2"  1位，由于99不精确，所以舍入为了100
console.log(num.toPrecision(2)); // "99"  2位
console.log(num.toPrecision(3)); // "99.0" 3位
```

本质上，toPrecision()方法会 根据数值和精度来决定调用 toFixed()还是 toExponential()。



## isInterger()方法 es6新加的

该方法为es6新加入的方法`Number.isInterger()`用来判断一个数字是否为整数。

```js
console.log(Number.isInteger(1)); // true 
console.log(Number.isInteger(1.00)); // true 1.0也算是整数噢 
console.log(Number.isInteger(1.01)); // false
```

