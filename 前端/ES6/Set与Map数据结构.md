# Set与Map数据结构

## Set

ES6提供了新的数据结构Set，**它类似于数组，但是成员的值都是唯一的，没有重复值。**

`Set`本身是一个构造函数，用来生成 Set 数据结构。

```js
const s = new Set();
[2, 3, 5, 4, 2, 2].forEach(x => s.add(x));
for(let i of s) {
	console.log(i);//2,3,5,4
}
//发现用add方法添加进去的s没有重复值
```



`Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```js
const set = new Set([1, 2, 3, 3, 3, 4, 4]);
console.log([...set]);//1,2,3,4
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
console.log(items.size);//5
```



## 利用 set的特性

利用set的特性获得了一种去除重复数组的方式

```js
[...new Set(array)]
```

上面的方法也可以用于，去除字符串里面的重复字符。

```js
[...new Set('abbbccc')].join('')
```

向 Set 加入值的时候，不会发生类型转换，所以`5`和`"5"`是两个不同的值。Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（`===`），**主要的区别是向 Set 加入值时认为`NaN`等于自身，而精确相等运算符认为`NaN`不等于自身。**

```js
let set2 = new Set();
let a = NaN;
let b = NaN;
set2.add(a);
set2.add(b);
console.log(set2);//就只有一个NaN

console.log(NaN === NaN);//false
```



## Set实例的属性和方法

Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

### Set 实例的操作方法

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身（因为返回集合的实例，所以可以链式调用进行添加）。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

```js
let s = new Set();
s.add(1).add(2).add(2);//注意添加了两次2
console.log(s.size);//2

console.log(s.has(1));//true
console.log(s.has(2));//true
console.log(s.has(3));//false

s.delete(2);
console.log(s);// 只有一个1了
console.log(s.has(2));//false
```



`Array.from`方法可以将 Set 结构转为数组

```js
//数组去重
function remdup(array) {
  return Array.from(new Set(array));
}
```



### Set 实例的遍历方法

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器(返回的是数组)
- `Set.prototype.forEach()`：使用回调函数遍历每个成员



需要特别指出的是，**`Set`的遍历顺序就是插入顺序。这个特性有时非常有用，比如使用 Set 保存一个回调函数列表，调用时就能保证按照添加顺序调用。**



`keys`方法、`values`方法、`entries`方法返回的都是遍历器对象（详见《Iterator 对象》一章）。由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

#### **（1）`keys()`，`values()`，`entries()`**

```js
let set = new Set(['red', 'green', 'blue']);
for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
   console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

//entries方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。
```



Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的`values`方法。

```js
Set.prototype[Symbol.iterator] === Set.prototype.values;//true
```

所以可以直接选择for...of替代values

```js
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```



#### （2）**`forEach()`**

该方法和数组那个一样，对每个成员进行一些操作，没有返回值。

```js
let s = new Set([1, 2, 3]);
s.forEach((value, key) => console.log(`${value} : ${key}`));
// 1 : 1
// 2 : 2
// 3 : 3
```

注意，Set特殊在，键名和键值一致，所以foreach的回调参数中的第一个和第二个参数一致。



#### （3）扩展运算符(...)

扩展运算符（`...`）内部使用`for...of`循环，所以也可以用于 Set 结构。

```
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```



由于可以转换成数组，所以数组的一系列方法也可以间接使用了。

例如`map`和`filter`方法

```js
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```





如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用`Array.from`方法。

```js
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```



## WeakSet

语法：

```js
const ws = new WeakSet(); 
```

弱集合中的**值只能是 Object 或者继承自 Object 的类型**，尝试使用非对象设置值会抛出 TypeError。

初始化实例：

```js
const val1 = {id: 1}, 
 val2 = {id: 2}, 
 val3 = {id: 3}; 
// 使用数组初始化弱集合
const ws1 = new WeakSet([val1, val2, val3]); 
alert(ws1.has(val1)); // true 
alert(ws1.has(val2)); // true 
alert(ws1.has(val3)); // true 
// 初始化是全有或全无的操作
// 只要有一个值无效就会抛出错误，导致整个初始化失败
const ws2 = new WeakSet([val1, "BADVAL", val3]); 
// TypeError: Invalid value used in WeakSet 
typeof ws2; 
// ReferenceError: ws2 is not defined 
// 原始值可以先包装成对象再用作值
const stringVal = new String("val1"); 
const ws3 = new WeakSet([stringVal]); 
alert(ws3.has(stringVal)); // true 
```





### WeakSet的add,has,delete方法

WeakSet的这些方法，和Set一致，一致，一致！



## Map

**Map与Object之间的区别**：

1. Map的键，可以是JavaScript中的任意一个类型，Object却只能是字符串
2. Map会严格维护键值对的插入顺序的，因此可以根据插入顺序执 行迭代操作。



JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```js
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content');//添加一个键为o，值为content
console.log(m.get(o));

console.log(m.has(o));//true
m.delete(o);
console.log(m.has(o));//false

```



作为构造函数，Map也可以接收一个数组作为参数。该数组的成员是一个一个表示键值对的数组。

```js
const map = new Map([
  ['name', '钱不二'],
  ['title', 'Author']
])
console.log(map.size);//2
console.log(map.has('name'));//true
console.log(map.get('name'));//钱不二
console.log(map.has('title'));//true
console.log(map.get('title'));//Author
```



事实上，不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构（详见《Iterator》一章）都可以当作`Map`构造函数的参数。这就是说，`Set`和`Map`都可以用来生成新的 Map。

```js
const set = new Set([
  ['foo', 1],
  ['bar', 2]
])
const m1 = new Map(set);
m1.get('foo');//1

//通过创建好的set 来创建一个map


//注意，连续多次对一个键赋值，后面的会覆盖掉前面的
m1.set(1, 'aaa');
m1.set(1, 'bbb');
console.log(m1.get(1));//bbb

```



注意：**只有对同一个对象的引用，Map结构才将其视为同一个键，这一点非常重要**

```js
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```



Map的键实际上跟内存地址绑定，只要内存不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。

如果Map的键是一个简单类型(数字，字符串，布尔值)，则只要两个值严格相等，Map就会视为同一个键

**另外，`undefined`和`null`也是两个不同的键。虽然`NaN`不严格相等于自身，但 Map 将其视为同一个键。**

```js
let map = new Map();

//0 和 -0严格相等所以是一个值
map.set(-0, 123);
console.log(map.get(0));//123

map.set(true, 1);
map.set('true', 2);
console.log(map.get(true));//1
console.log(map.get('true'));//2

map.set(undefined, 3);
map.set(null, 4);
console.log(map.get(undefined));//3

// NaN比较特殊 虽然严格相等 === 不一致，但是map还是将他归为一个键
map.set(NaN, 123);
map.set(NaN, 456);
console.log(map.get(NaN));//456
```





### Map实例的属性和操作方法

1、**size 属性**

返回Map结构成员总数。

```js
			const map = new Map();
			map.set('foo', true);
			map.set('bar', false);
			
			console.log(map.size);//2
```

2、**Map.prototype.set(key, value)**

`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

```js
const m = new Map();
m.set('edition', 6);
m.set(262, 'standard');
console.log(m.set(undefined, 'nah'));//返回整个map

//由于set返回一个map对象，所以可以链式调用
m.set(1, 'a')
.set(2, 'b')
.set(3, 'c')
```



3、**Map.prototype.get(key)**

`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

```js
const m = new Map();
const hello = function() {
  console.log('hello');
}
// 键是一个函数
m.set(hello, 'hello world');
console.log(m.get(hello));//hello world
```



4、**Map.prototype.has(key)**

`has`方法返回一个布尔值，表示某个**键**是否在当前 Map 对象之中。

```js
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true
```



5、**Map.prototype.delete(key)**

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

```
const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false
```



6、**Map.prototype.clear()**

`clear`方法清除所有成员，没有返回值。

```js
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```



### Map实例遍历方法

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。

map默认遍历器接口，就是entries方法

```
map[Symbol.iterator] === map.entries
// true
```

- `Map.prototype.forEach()`：遍历 Map 的所有成员。



```js
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```



Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

```js
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```



## weakMap

语法：

```js
const wm = new WeakMap();
```

特点：弱映射中的**键只能是 Object 或者继承自 Object 的类型**，尝试使用非对象设置键会抛出 TypeError。**值的类型没有限制**。



初始化示例：

```js
const key1 = {id: 1},
      key2 = {id: 2},
      key3 = {id: 3};
// 使用嵌套数组初始化弱映射
const wm1 = new WeakMap([ 
 [key1, "val1"], 
 [key2, "val2"], 
 [key3, "val3"] 
]); 
alert(wm1.get(key1)); // val1 
alert(wm1.get(key2)); // val2 
alert(wm1.get(key3)); // val3 
// 初始化是全有或全无的操作
// 只要有一个键无效就会抛出错误，导致整个初始化失败
const wm2 = new WeakMap([ 
 [key1, "val1"], 
 ["BADKEY", "val2"], 
 [key3, "val3"] 
]); 
// TypeError: Invalid value used as WeakMap key 
typeof wm2; 
// ReferenceError: wm2 is not defined 
// 原始值可以先包装成对象再用作键
const stringKey = new String("key1"); 
const wm3 = new WeakMap([ 
 stringKey, "val1" 
]); 
alert(wm3.get(stringKey)); // "val1" 
```



### weakMap的set,get,has方法

weakMap的这三个方法包括delete方法，都是和上面的Map数据类型方法一致，不能说是很像，只能说是一模一样。



### weakMap浅析

WeakMap 中“weak”表示弱映射的键是“弱弱地拿着”的。意思就是，这些键不属于正式的引用， 不会阻止垃圾回收。但要注意的是，弱映射中值的引用可不是“弱弱地拿着”的。只要键存在，键/值 对就会存在于映射中，并被当作对值的引用，因此就不会被当作垃圾回收。

可以这样理解：弱映射，由于键，必须的Object类型，是一个引用值，当这个引用值被垃圾回收掉后，这个映射也会相继消失

例如：

```js
const wm = new WeakMap(); 
wm.set({}, "val");
// 由于 {} 没有任何人引用，所以，代码执行完毕后，立刻就被垃圾回收了，相应的这个键/值对也消失了
```



另一个例子：

```js
const wm = new WeakMap(); 
const container = { 
 key: {} 
}; 
wm.set(container.key, "val"); 
function removeReference() { 
 container.key = null; 
}
```

这一次，container 对象维护着一个对弱映射键的引用，因此这个对象键不会成为垃圾回收的目 标。不过，如果调用了 removeReference()，就会摧毁键对象的最后一个引用，垃圾回收程序就可以 把这个键/值对清理掉。

>例子来自（《JavaScript高级程序设计第6章》）
