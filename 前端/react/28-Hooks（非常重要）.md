# Hooks（非常重要）

`Hooks`的出现大大的提高了函数式组件的功能。

可以让函数式组件中使用`state`以及其它的`React`特性。

## Hook 就是 JavaScript 函数，但是使用它们会有两个额外的规则：

- 只能在**函数最外层**调用 Hook。不要在循环、条件判断或者子函数中调用。
- 只能在 **React 的函数组件**中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中，我们稍后会学习到。）





## `State Hook: React.useState()`

作用：State Hook可以让函数式组件也可以拥有`state`状态，并进行状态数据的读写操作。

语法：

```jsx
const [xxx, setXxx] = React.useState(initValue)  
```

参数：`initValue` 第一次初始化指定的值在内部作缓存

返回值：包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数



>返回的`setXxx`的2中写法：(特别像类式组件中的`this.setState`)

写法1：`setXxx(newValue): `参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值

示例：

```jsx
const [count, setData] = React.useState(0);
const add = ()=> {
  setData(count+1);
}
```



写法2：`setXxx(value => newValue): `参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值

示例：

```jsx
  const [count, setData] = React.useState(0);
  const add = ()=> {
    // setData(count+1);
    setData((count) => {
      return count + 1;
    })
  }
```



<blockquote 
style="
background-color: rgba(255,229,100,0.3);
border-left-color: #ffe564;
font-size: 20px
">类式组件和函数式组件的对比</blockquote>

```jsx
import React, { Component } from 'react'

// export default class Demo extends Component {
//   state = {count: 0}
//   add = ()=> {
//     this.setState((state)=> {
//       return {count: state.count+1}
//     })
//   }
//   render() {
//     return (
//       <div>
//         <h2>当前求和为：{this.state.count}</h2>
//         <button onClick={this.add}>点我+1</button>
//       </div>
//     )
//   }
// }


// 函数式组件
export default function Demo(props) {
  console.log('demo')
  const [count, setData] = React.useState(0);
  const add = ()=> {
    // setData(count+1);
    setData((count) => {
      return count + 1;
    })
  }

  return (
    <div>
      <h2>当前求和为：{count}</h2>
      <button onClick={add}>点我+1</button>
    </div>
  )
}
```



## `Effect Hook`

`Effect Hook`：可以让你在函数组件中执行副作用操作（比如发送网络请求，手动变更 DOM，记录日志）



语法：

```jsx
import {useEffect} from 'react'
useEffect(() => { 
          // 在此可以执行任何带副作用操作
          return () => { // 在组件卸载前执行
            // 在此做一些收尾工作, 比如清除定时器/取消订阅等
          }
        }, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
```

特点：

可以把 `useEffect Hook `看做如下三个函数的组合
```jsx
componentDidMount()// dom挂载时调用
componentDidUpdate()// 组件更新时调用
componentWillUnmount()// 组件将要卸载时调用
```



>`componentDidMount()`相当于：

```js
useEffect(() => {
  xxx
}, [])
// 注意最后这个空数组一定要写，表示谁都不监视，带上空数组后这里回调只会在dom挂载时调用一下
```

>`componentDidUpdate()`：相当于

```jsx
useEffect(() => {
  xxx
}, [count, ...])
// 指定第二个参数监视的state，当count更新时，回调函数会进行调用
```


>`componentWillUnmount()`:相当于

```js
useEffect(() => {
  xxx
  return () => {
    ....// 这里第二个回调，就和componentWillUnmount()的功能类似
  }
},[])
```





## `Ref Hook`

`Ref Hook`：可以让你在函数式组件中获取`ref`

语法：

```jsx
import {useRef} from 'react'
const refContainer = useRef();
```

作用：和类式组件中的`createRef()`功能一样，都是创建一个位置存储`ref`并且**专人专用**



示例：

```jsx
import {useRef} from 'react'
export default function Demo(props) {
  const myRef = useRef();
  const show = () => {
    alert(myRef.current.value)
  }

  return (
    <div>
      <input type="text" ref={myRef}/>
      <button onClick={show}>展示input内容</button>
    </div>
  )
}
```



