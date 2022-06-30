# TypeScript的基本类型

## 一、类型声明

类型声明是`TS`的一个非常重要的特点，通过类型声明可以给变量声明类型



## 两种类型

比较特殊的：

1. `any`（尽力避免any）

`any`类型就是ts不能帮我们进行类型推断，也不能帮我们查找出错误，常见在一些函数中，一些函数根据不同输入值，会有不同的返回值，`ts`这时就会将这个函数的返回值当作`any`类型

2. `void`

`void`一般表示一个函数没有返回值，当函数返回`undefined`或者`null`时，也不会报错

3. `never`

`never`也是修饰函数的一种类型，表示这个函数永远不会执行到底部，也就是中途会报错呀等等，只有我们真正的不希望函数返回东西，才需要`never`

```tsx
const throwError = (message: string): never => {
	throw new Error(message);
  

};
```



一种原始数据类型：`number`、`boolean`、`string`、`void`、`undefined`、`null`、`symbol`、

另一种引用类型：`funcitons`、`arrays`、`classes`、`objects`

![image-20220629084040958](../../../../../AppData/Roaming/Typora/typora-user-images/image-20220629084040958.png)



类型的作用：

1. 可以让typescript的编译器识别出代码中的错误
2. 能让程序员更好的理解变量在各个模块之间的流动，让代码更易懂

