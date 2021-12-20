# for..in和for...of和for...each区别

## for..in和for..of

首先for...of是es6出现的新接口，为迭代器设计的，

## for...of和for...in的区别

```js
        const xiyou = ['唐僧','猪八戒','孙悟空','沙僧'];
        // 用for...of 遍历数组
        for(let v of xiyou) {
            console.log(v);//唐僧, 猪八戒...
        }
        // 用for...in遍历数组
        for(let v in xiyou) {
            console.log(v);//0,1,2,3
        }

/*for...of 中间的遍历v是键值(数组的值)*/
/*for...in 中间的遍历v是键名(数组的索引)*/
```

1、for...of只能用在可迭代的对象上，获取的是迭代器所返回的`value`值，而`for...in`可以获取所有对象的键名。

2、`for...in`会遍历对象的整个原型链，性能非常差不推荐使用，而`for...of`只遍历当前对象不会遍历它的原型链

3、对于数组的遍历`for...in`会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，`for...of`则只会返回数组下标所对应的属性值

