# Map

作为 ECMAScript 6 的新增特性，Map 是一种新的集合类型，为这门语言带来了真正的键/值存储机 制。Map 的大多数特性都可以通过 Object 类型实现，但二者之间还是存在一些细微的差异。具体实践 中使用哪一个，还是值得细细甄别。



## 区别一：

Object只能用数值和string作为键名

**Map可以用任意类型作为键名**

## 创建一个Map映射

第一种：利用new关键字和Map的构造函数可以直接创建一个空映射

语法：

```
const m = new Map(参数);
```

如果想要初始化实例，可以在参数中传递一个可迭代对象，需要包含键/值对数组

```js
const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
console.log(m1);
```

第二种：使用自定义迭代器初始化映射

```
const m2 = new Map({ 
 [Symbol.iterator]: function*() { 
 yield ["key1", "val1"]; 
 yield ["key2", "val2"]; 
 yield ["key3", "val3"]; 
 } 
}); 
alert(m2.size); // 3 
```



## 基本方法

### set()方法

set()方法用于往映射中添加键值对

由于set()方法返回整个映射，所以可以链式添加set().set()

```js
const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
// 链式调用
    m1.set('firstName', 'qian')
      .set('lastName', 'buer');
    console.log(m1);
    /*
    Map(5)
[[Entries]]
0: {"key1" => "val1"}
1: {"key2" => "val2"}
2: {"key3" => "val3"}
3: {"firstName" => "qian"}
4: {"lastName" => "buer"}
size: 5
[[Prototype]]: Map
*/
```



### has(键名)方法

has(参数)方法用来检查映射中是否含有指定**键名**，返回一个布尔值

```js
    const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
    console.log(m1.has('firstName'));// false
    m1.set('firstName', 'qian')
      .set('lastName', 'buer');
    console.log(m1.has('firstName')); // trues
    console.log(m1);
```



### get(键名)方法

get()方法用于获得指定键名对应的**键值**

```js
    const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
    console.log(m1.has('firstName'));
    m1.set('firstName', 'qian')
      .set('lastName', 'buer');
    console.log(m1.get('key1')); // val1
    console.log(m1.get('firstName')); // qian
    console.log(m1.get('firstName2')); // undefined
    console.log(m1);
```

### delete(键名)方法

delete()方法用于删除指定键值对，

```js
    const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
    console.log(m1.has('firstName'));
    m1.set('firstName', 'qian')
      .set('lastName', 'buer');
    console.log(m1.has('qian'));
    m1.delete('lastName');// 删除了lastName键值对
    console.log(m1);
```

### clear()方法清空

clear()方法直接清空整个映射

```js
    const m1 = new Map([
      ['key1', 'val1'],
      ['key2', 'val2'],
      ['key3', 'val3']
    ])
    console.log(m1.has('firstName'));
    m1.set('firstName', 'qian')
      .set('lastName', 'buer');
    console.log(m1.has('qian'));
    m1.clear();
    console.log(m1.size);// 0
```



## 区别二：

与 Object 类型的一个主要差异是，Map 实例会维护键值对的插入顺序，因此可以根据插入顺序执 行迭代操作。

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

[...map.keys()]s
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

## 选择Object还是Map



如果需要频繁的删除对象中的内容，则选择Map

其余他俩差距不大