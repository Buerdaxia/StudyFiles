# Redux

注意，这个Redux是一个公用的库，不仅能用在react上也可以在别的框架上使用。

概念：redux是一个专门用于做**状态管理**的JS库(类似于vuex)

作用：集中式管理react应用中多个组件**共享**的状态

总体原则：**能不用就不用，如果不用比较麻烦，可以考虑使用**



![7-redux原理图](../../前端图片/react/7-redux原理图.png)

## action

概念：**action** 是一个具有 `type` 字段的普通 JavaScript 对象。**你可以将 action 视为描述应用程序中发生了什么的事件**.

1. 包含2个属性

type：标识属性, 值为字符串, 唯一, 必要属性

data：数据属性, 值类型任意, 可选属性

例子：

```jsx
// 一般都会写出函数方式
function increment(value) {
  return {
    type: 'increment',
    data: value
  }
}
```

## store

1. 将state、action、reducer联系在一起的对象

如何得到此对象?

1) import {createStore} from 'redux'

2) import reducer from './reducers'

3) const store = createStore(reducer)

此对象的功能?

1) getState(): 得到state

2) dispatch(action): 分发action, 触发reducer调用, 产生新的state

3) subscribe(listener): 注册监听, 当产生了新的state时, 自动调用

示例：

```jsx
/*
  该文件专门用于暴露一个store对象，整个应用只有一个store
*/

// 引入createStore，专门用于创建redux最为核心的store
import { createStore } from 'redux';
// 引入为count组件服务的reducer
import countReducer from './count_reducer';

// 暴露store
export default createStore(countReducer);

```



## reducer

1. 用于**初始化状态**、**加工状态**。

2. 加工时，根据旧的state和action， 产生新的state的**纯函数**。

注意：第一次会由store默认调用,传递的`action`：

```jsx
{type:'@@INit_xx.xx.xx.x'}
```

示例：

```jsx
import { INCREMENT, DECREMENT } from './constant';
/*
  1.该文件是一个用于创建服务Count组件的reducer，reducer本质就是一个函数
  2. reducer函数接收两个参数，分别为：之前的状态（preState）,动作对象（action）
  */
const initState = 0;// initState = {value:0}
export default function countReducer(preState = initState, action) {
	// 从action对象中获取type,data
	const { type, data } = action;
	// 根据type决定如何加工对象
	switch (type) {
		// 如果是加法
		case INCREMENT:
			return preState + data;

		// 如果是减法
		case DECREMENT:
			return preState - data;

		// 初始化时
		default:
			return preState;
	}
}

```





## 什么时候使用？

1. 某个组件的状态，需要让其他组件可以随时拿到（共享）。
2. 一个组件需要改变另一个组件的状态（通信）。



## 顶层API

官网有，！比我写的清晰多了

有三个常用

1. createStore
2. applyMiddleware
3. combineReducers



## store身上的API

具体的官网有

### getState()

功能：返回应用当前的 state 树。它与 store 的最后一个 reducer 返回值相同。



### dispatch(action)

功能：分发 action。这是触发 state 变化的惟一途径。

将使用当前 `getState()`的结果和传入的 `action` 以同步方式的调用 store 的 reduce 函数。它的返回值会被作为下一个 state。从现在开始，这就成为了 `getState()` 的返回值，同时变化监听器(change listener)会被触发。



### subscribe(listener)

功能：添加一个变化监听器。每当 dispatch action 的时候就会执行，state 树中的一部分可能已经变化。你可以在回调函数里调用 `getState()`来拿到当前 state。

示例：

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import store from './redux/store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App></App>);

// 监听redux修改,一旦修改重新渲染整个容器
store.subscribe(() => {
  // 原因react不会监听redux的修改，所以我们修改后需要手动重新渲染界面
	root.render(<App></App>);
});

```





## 异步action

备注：异步action其实并不是必要的，完全可以我们自己等待异步任务完成后调用同步的action。

使用场景：想要对状态（store）进行操作，但是具体的数据需要异步任务来返回，就要使用异步action了

前提：需要安装`redux-thunk`中间件

>第一步：安装`redux-thunk`

```
npm install redux-thunk
```

>第二步：在store中使用`redux-thunk`

```js
/*
  该文件专门用于暴露一个store对象，整个应用只有一个store
*/

// 引入createStore，专门用于创建redux最为核心的store
import { createStore, applyMiddleware } from 'redux';
// 引入为count组件服务的reducer
import countReducer from './count_reducer';

// 引入redux-thunk用于支持异步action
import thunk from 'redux-thunk';
// 暴露store，并加载中间件
export default createStore(countReducer, applyMiddleware(thunk));

```

`applyMiddleware`：专门用来加载中间件的



>第三步：就可以在action中编写异步任务了
>
>注意：这里我们用定时器模仿了一下异步任务

```js
import { INCREMENT, DECREMENT } from './constant';

/*
  该文件专门为Count组件生成action对象
*/

// 同步action,指action的指为Object类型的一般对象
export function createIncrementAction(data) {
	return { type: INCREMENT, data };
}

export function createDecrementAction(data) {
	return { type: DECREMENT, data };
}

// 异步action,指action的值为函数,异步action中一般都会调用同步action
export function createIncrementAsyncAction(data, time) {
	return dispatch => {
		setTimeout(() => {
			dispatch(createIncrementAction(data));
		}, time);
	};
}

```

