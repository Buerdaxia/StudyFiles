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

