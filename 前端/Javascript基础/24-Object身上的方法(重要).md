# Object身上的方法

## 可枚举和不可枚举

对象的属性的可枚举性和不可枚举性是由属性的**enumerable**值决定的，true为可枚举，false为不可枚举

不可枚举：JS中预定义的原型属性一般是不可枚举的

可枚举：自己定义的属性一般可枚举

## 方法1：**`Object.getOwnPropertyNames()`**

语法：`Object.getOwnPropertyNames(参数)`

该方法方法返回一个由指定对象的**所有自身属性的属性名**（**包括不可枚举属性**但不包括Symbol值作为名称的属性）**组成的数组**。

示例：

```js
let obj = {
  a: '123',
  qian: '12345',
  hahah: '123'
}
console.log('obj', Object.getOwnPropertyNames(obj));
// 输出:['a', 'qian', 'hahah']
```

应用场景：当需要一个对象的属性名所组成的数组时可以使用该方法。



## 方法2：Object.keys()

语法：`Object.keys(obj)`

方法会返回一个由一个给定**对象的自身可枚举属性**组成的**数组**，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

示例：

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']
```



## 方法3：**Object.values()**

语法：`Object.values(obj)`

`Object.values()`返回一个数组，其元素是在对象上找到的**可枚举属性值**。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

示例：

```js
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']
```



## 方法4：**Object.entries()**

语法：`Object.entries(obj)`

该方法可以看作是方法2，方法3的结合版本，将键值对作为一个小数组，一个小数组的形式放到一个大数组中。

官方描述：`Object.entries()`返回一个数组，其元素是与直接在`object`上找到的可枚举属性键值对相对应的数组。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

示例：

```js
const obj = { foo: 'bar', baz: 42 };
// 变成由一个个键值对儿组成的小数组
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

// array like object with random key ordering
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
```





## 方法5：**Object.assign()**

语法：`Object.assign(目标对象, 源对象)`

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

注意：**针对深拷贝，需要使用其他办法，因为 `Object.assign()`拷贝的是（可枚举）属性值。**假如源值是一个对象的引用，它仅仅会复制其引用值。（也就是只能是一层的对象）

示例：

```js
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```



## 方法6：**hasOwnProperty()**

语法：`obj.hasOwnProperty(prop)`

所有继承了 [`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 的对象都会继承到 `hasOwnProperty` 方法。这个方法可以用来检测一个对象是否含有特定的自身属性；和 [`in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in) 运算符不同，**该方法会忽略掉那些从原型链上继承到的属性**。返回一个**布尔值**

注意：**即使属性的值是 `null` 或 `undefined`，只要属性存在，`hasOwnProperty` 依旧会返回 `true`。**

示例：

```js
let obj2 = {
  a: '1',
  b: '2',
  123: '3'
  d: null
}
console.log('obj2', Object.keys(obj2));

console.log('hasOwnProperty', obj2.hasOwnProperty('c'));// false
console.log('hasOwnProperty', obj2.hasOwnProperty('a'));// true
console.log('hasOwnProperty', obj2.hasOwnProperty('d'));// true

o = new Object();
o.hasOwnProperty('prop'); // 返回 false
o.prop = 'exists';
o.hasOwnProperty('prop'); // 返回 true
delete o.prop;
o.hasOwnProperty('prop'); // 返回 false
```
