# TS中的as关键字

类型断言：类型断言只能够“欺骗”TS检查器，并不能真正改变它本身

as是TS中的一个关键字，简单来说就是让TS来相信我指定的值。



示例：

```ts
// 就是name类型为number
const name = get('name') as number;
```

