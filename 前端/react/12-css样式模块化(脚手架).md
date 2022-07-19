# 样式模块化(脚手架)

不采用模块化的后果：如果出现选择器名称一样会有覆盖效果出现。



## 方式

> 首先是纯`css`



>第一步：

创建css文件时，加上`.module`

例如：

```jsx
// 原来名称
Hello.css
// 修改过后的名称
Hello.module.css
```





>第二步：引入



```js
// 原来引入方式
import './Hello.css'
// 修改过后的
import hello from './Hello.module.css'
```



>第三步：使用

```jsx
import React, { Component } from 'react';
// 引入样式文件
import hello from './Hello.module.css'

export default class Hello extends Component {
	render() {
    {/*采用对象的形式访问*/}
		return <h2 className={hello.title}>Hello world</h2>;
	}
}

```

