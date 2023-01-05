# 命名空间namespace

在工作中，总是会无法避免全局变量的污染，而TS中解决全局变量污染问题的方式是：namespace



体现：

你在一个文件夹下创建2个TS文件，然后声明两个相同的变量，这时你就会发现报错了，说不能声明同名变量，这是应为此时这两个TS文件都是在全局环境。

解决上面的问题的两个方式：

1. export、import局部模块，形成局部作用域
2. 使用namespace



namespace作用：

* 内部模块，主要用于组织代码避免冲突
* 命名空间的类默认私有
* 通过export进行暴露
* 通过namespace关键字定义



**TS和ES6是一样的，任何包含顶级import或者export的文件都会被当成一个模块。相反，如果一个文件不带有顶级import或者export声明，那么它的内容被视为全局可见的**



## 基础使用



示例：

```ts
// 声明
namespace A {
  export const a: number = 2
}


// 使用
console.log(A.a);
```

其实namespace本质就是利用对象+包了一层函数实现的

上面代码编译成js：

```js
// 声明
var A;
(function (A) {
    A.a = 2;
})(A || (A = {}));
// 使用
console.log(A.a);

```





## 嵌套命名空间

namespace是可以嵌套的，使用时也是一层一层的

示例：

```ts
// 嵌套声明
namespace B {
  export namespace C {
    export const a :number = 3;
  }
}

// 嵌套使用
console.log(B.C.a)
```





## 抽离命名空间（export、import）

命名空间也是可以通过export暴露，import引入

示例：

```ts
// 暴露
export namespace Q {
  export const name : string = '钱不二'
}

// 引入
import {Q} from './index2'
// 使用
console.log(Q.name);
```







## 给命名空间起别名儿

当命名空间层级太多时，使用起来就很不方便，我们可以使用`import`关键字来给一个命名空间起别名儿，来简化使用

示例：

```ts
// 嵌套声明
namespace B {
  export namespace C {
    export const a :number = 3;
  }
}

// 起别名
import alias = B.C.a

// 嵌套使用
console.log(B.C.a)
// 别名使用
console.log(alias)
```





## 命名空间的合并

这个合并和interface的合并是一样的，如果有两个同名的namespace出现，会将两个namespace内容合并起来



示例：

```ts
namespace A {
  export const a = 0;
}

namespace A {
  export const b = 1;
}


// 自动合并
namespace A {
  export const a = 0;
  export const b = 1;
}
```

