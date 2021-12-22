# for-in与for-of

## for-in语句

for-in语句是一种严格的迭代语句，**用于枚举对象中的非符号键属性**

```js
obj = {
  name: '钱不二',
  age: 18,
  sex: '男'
}
for (const item in obj) {
    console.log(item);
}

//name , age, sex
```

注意：ECMAScript中的对象的属性是无序的，因此for-in语句不能保证返回对象属性的顺序，但是会将所有可枚举的属性都返回一次。

如果 for-in 循环要迭代的变量是 null 或 undefined，则不执行循环体。

## for-of

for-of语句也是一种严格的迭代语句，**用于遍历可迭代对象的元素（例如数组就是一个可迭代对象）**

```js
for (const el of [2, 3, 4, 5]) {
  console.log(el);
}
//2, 3, 4, 5
```

注意：for-of循环会按照可迭代对象的next()方法产生值的顺序迭代元素。后续看完第七章过来补充



如果尝试迭代变量不支持迭代，则会抛出错误。