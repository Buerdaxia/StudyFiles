# enum-TS中的新东西(不是很重要)

enum：本意为枚举的意思。

概念：用来表示一些联系非常密切变量的集合。

基本语法：

```ts
enum 名称 {
  xxx = '值1',
  xxx = '值2'...
}
```

![14-ts中的enum](../../前端图片/typescript/14-ts中的enum.PNG)

作用：使用枚举并不会提升性能（转换成`js`时是一个对象），**主要的功能是它在TS中是向其他工程师给予提示这些变量具体的作用，来帮助工程师来阅读代码的**。



## 何时使用它

一些值它不常变化，并且值本身的含义比较难理解，我们可以用enum来描述一下。

```ts
// 例如 有时
// HomeWin指主场胜利，AwayWin指客场胜利，Draw是平局
enum MatchResult {
	HomeWin = 'H',
	AwayWin = 'A',
	Draw = 'D'
}

let manUnitedWins = 0;
for (let match of matches) {
  // 原本这里判断写的是 match[5] === 'H' 这样就不好理解这句代码具体的含义是什么
	if (match[1] === 'Man United' && match[5] === MatchResult.HomeWin) {
		manUnitedWins++;
	} else if (match[2] === 'Man United' && match[5] === MatchResult.AwayWin) {
		manUnitedWins++;
	}
}
```

