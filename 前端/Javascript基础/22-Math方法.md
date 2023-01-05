# Math方法

## Math对象属性

| 属性      | 说明                       |
| --------- | -------------------------- |
| Math.E    | 自然对数的基数e的值        |
| Math.LN10 | 10为底的自然对数           |
| Math.LN2  | 2为底的自然对数            |
| Math.PI   | pai的值（3.1415...的那个） |



## min()和max()方法

min()和max()方法用于确定一组数值中的最小值和最大值。

语法：

```
let max = Math.max(一组数值);
let min = Math.min(一组数值);
```



代码实例：

```js
let max = Math.max(3, 54, 32, 16);
console.log(max);//54

let min = Math.min(3, 54, 32, 16);
console.log(min);//3
```



小技巧如果想要数组中的最大值和最小值！可以配合扩展运算符

```js
let values = [1, 2, 3, 4, 5, 6, 7, 8]; 
let max = Math.max(...val); // 8
```



## Math的舍入方法

*  Math.ceil()方法始终向上舍入为最接近的整数。
*  Math.floor()方法始终向下舍入为最接近的整数。
*  Math.round()方法执行四舍五入。
*  Math.fround()方法返回数值最接近的单精度（32 位）浮点值表示。

代码示例：

```js
console.log(Math.ceil(2.5));//3
console.log(Math.floor(2.5));//2
console.log(Math.round(2.5));//3
console.log(Math.fround(2.5));//2.5
```



**四舍五入公式**：

```js
Math.round(value * num) / num;

// value: 被舍入的值
// num	: 舍入位数，1位小数：10，2位小数：100，三位小数：1000 ...
```







## random()方法

Math.random()方法返回一个0~1范围内的随机数，**其中包含0但是不包含1**

**这个方法有一个比较重要的公式：**

```js
let number = Math.floor(Math.random() * total_number_of_choices + first_possible_value) 
// 从first_possible_value 第一个数
// total_number_of_choices 可选总数

```

例如，想从1~10范围内（**包含1和10**）随机选择一个数字

```
let num = Math.floor(Math.random() * 10 + 1);
```

分装一个函数

```js
function selectFrom (lowerValue, upperValue) {
  // 得到可选总数
  let choices = upperValue - lowerValue + 1;
  
  return Math.floor(Math.random() * choices + lowerValue);
}

let num = selectFrom(2, 10);
console.log(num);// 得到2~10范围内的值，并且包含2和10
```



小技巧：利用上面的函数，我们可以随机从一个数组中抽取一项出来

```js
let arr = ['blue', 'black', 'white', 'red'];
let color = arr[selectFrom(0, arr.length - 1)];
console.log(color); //就是随机一项
```





## Math的其他方法

**带有*号的是平常可能会用到的，其他用的非常少，甚至不用。**

| 方法                | 说明                             |
| ------------------- | -------------------------------- |
| *Math.abs(x)        | 返回 x 的绝对值                  |
| *Math.exp(x) 返回   | Math.E 的 x 次幂                 |
| Math.expm1(x)       | 等于 Math.exp(x) - 1             |
| *Math.log(x)        | 返回 x 的自然对数                |
| Math.log1p(x)       | 等于 1 + Math.log(x)             |
| *Math.pow(x, power) | 返回 x 的 power 次幂             |
| Math.hypot(...nums) | 返回 nums 中每个数平方和的平方根 |
| Math.clz32(x)       | 返回 32 位整数 x 的前置零的数量  |
| Math.sign(x)        | 返回表示 x 符号的 1、0、-0 或-1  |
| Math.trunc(x)       | 返回 x 的整数部分，删除所有小数  |
| *Math.sqrt(x)       | 返回 x 的平方根                  |
| Math.cbrt(x)        | 返回 x 的立方根                  |
| Math.acos(x)        | 返回 x 的反余弦                  |
| Math.acosh(x)       | 返回 x 的反双曲余弦              |
| Math.asin(x)        | 返回 x 的反正弦                  |
| Math.asinh(x)       | 返回 x 的反双曲正弦              |
| Math.atan(x)        | 返回 x 的反正切                  |
| Math.atanh(x)       | 返回 x 的反双曲正切              |
| Math.atan2(y, x)    | 返回 y/x 的反正切                |
| *Math.cos(x)        | 返回 x 的余弦                    |
| *Math.sin(x)        | 返回 x 的正弦                    |
| *Math.tan(x)        | 返回 x 的正切                    |

