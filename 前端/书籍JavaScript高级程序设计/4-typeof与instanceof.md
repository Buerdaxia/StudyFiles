# typeof与instanceof

## typeof

```js
let s = "Nicholas"; 
let b = true; 
let i = 22; 
let u; 
let n = null; 
let o = new Object(); 
console.log(typeof s); // string 
console.log(typeof i); // number 
console.log(typeof b); // boolean 
console.log(typeof u); // undefined 
console.log(typeof n); // object 
console.log(typeof o); // object 
```

typeof最适合用来判断一个变量是否为原始类型。更确切地说，它是判断一 个变量是否为string、number、boolean或 undefined 的最好方式。如果值是对象或 null，会返回object

注意：typeof也能判断function，应为ECMA-262 规定，任何实现内部[[Call]]方法的对象都应该在 typeof 检测时返回"function"。



## instanceof

```js
//语法
result = variable instanceof constructor

console.log(person instanceof Object); // 变量 person 是 Object 吗？
console.log(colors instanceof Array); // 变量 colors 是 Array 吗？
console.log(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```

如果变量是给定引用类型的实例，则会返回true

**该方法会按照原型链一直查找，如果能找到返回true，否则返回false。**

按照定义，所有引用值都是 Object 的实例，因此通过 instanceof 操作符检测任何引用值和 Object 构造函数都会返回 true。类似地，如果用 instanceof 检测原始值，则始终会返回 false， 因为原始值不是对象