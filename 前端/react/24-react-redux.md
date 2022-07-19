# react-redux

![8-react-redux模型图](../../前端图片/react/8-react-redux模型图.png)



注意：容器组件，不是我们自己写的哦，是react-redux创建的呦



## 基本使用

1）首先要明确两个概念：

​		一：UI组件：不能使用任何redux的API，只负责页面呈现、交互等。

​		二：容器组件：负责和redux通讯，将结果交给UI组件。

2）如何创建一个容器组件？需要react-redux的connect函数

>首先：创建一个和components文件夹同级的container文件夹

>第二步：创建一个UI组件的容器组件

>第三步，使用connect函数

语法：`connect(mapStateToProps, mapDispatchToProps)(UI组件);`

-`mapStateToProps`：映射状态，返回值是一个对象。

-`mapDispatchToProps`：映射操作状态的方法，返回值也是一个对象。

示例：(`container/Count/index.jsx`)

```js
// 引入Count组件
import CountUI from '../../components/Count';

// 引入connect用于链接UI组件与redux
import { connect } from 'react-redux';

import {createIncrementAction,createDecrementAction,createIncrementAsyncAction} from '../../redux/count_action'

/*  1.mapStateToProps返回的是一个对象
    2.返回对象中的key就作为UI组件props的key，value就作为UI组件props的value--状态
    2.mapStateToProps用来传递redux的状态
  */ 
function mapStateToProps(state) {
  return {count: state}
}

/*  1.mapDispatchToProps返回的是一个对象
    2.对象中的key就作为UI组件props的key，value就作为UI组件props的value--操作状态的方法
    3.mapDispatchToProps函数用来传递操作状态的方法

  */ 
function mapDispatchToProps(dispatch) {
  return {
    increment: (number)=>{
      // 通知redux执行加法
      dispatch(createIncrementAction(number*1));
    },
    decrement: (number)=>{
      dispatch(createDecrementAction(number*1));
    },
    incrementAsync: (number, time)=>{
      dispatch(createIncrementAsyncAction(number*1,time));
    }
  }
}

// 创建并暴露一个Count的容器组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI);


// 简化写法
// 引入Count组件
import CountUI from '../../components/Count';

// 引入connect用于链接UI组件与redux
import { connect } from 'react-redux';

import {createIncrementAction,createDecrementAction,createIncrementAsyncAction} from '../../redux/count_action'

// 创建并暴露一个Count的容器组件
export default connect(
  state => ({count: state}),
  // mapDispatchToProps一般写法
  // dispatch => (
  //   {
  //   increment: (number)=>{
  //     // 通知redux执行加法
  //     dispatch(createIncrementAction(number));
  //   },
  //   decrement: (number)=>{
  //     dispatch(createDecrementAction(number));
  //   },
  //   incrementAsync: (number, time)=>{
  //     dispatch(createIncrementAsyncAction(number,time));
  //   }
  //   }
  // )

  // mapDispatchToProps简写
  {
    increment:createIncrementAction,
    decrement:createDecrementAction,
    incrementAsync:createIncrementAsyncAction
  }
)(CountUI);
```



3）备注：容器组件中的store是靠props传入的，而不是在容器组件中直接引入（使用`Provider`可以不用写了）

4）使用`Provider`优化上面备注：

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import store from './redux/store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	// 使用Provider来帮助我们传递store
	<Provider store={store}>
		<App></App>
	</Provider>
);

```



## connect方式，优化版本

核心：将UI组件和容器组件写在一起，然后将容器组件暴露出去

`containers/Count/index.jsx`:

```jsx
import React, { Component } from 'react';
// 引入connect用于链接UI组件与redux
import { connect } from 'react-redux';

// 引入action
import {createIncrementAction,createDecrementAction,createIncrementAsyncAction} from '../../redux/count_action'


// 下面是UI组件
class Count extends Component {
	// state = { count: 0 }

	// 加法
	increment = () => {
		const { value } = this.selectNumber;
    this.props.increment(value*1)
	};

	// 减法
	decrement = () => {
		const { value } = this.selectNumber;
    this.props.decrement(value*1);
	};

	// 奇数再加
	incrementIfOdd = () => {
		const { value } = this.selectNumber;
    const{count} = this.props;
    if(count % 2 !== 0) {
      this.props.increment(value*1);
    }
	};

	// 异步加
	incrementAsync = () => {
		const { value } = this.selectNumber;
    this.props.incrementAsync(value*1, 500);
	};

	render() {
    // console.log('UI组件接收的props',this.props);

		return (
			<div>
				<h1>当前求和为: {this.props.count}</h1>&nbsp;
				<select ref={c => (this.selectNumber = c)}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>
				&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>
			</div>
		);
	}
}

// 创建并暴露Count的容器组件
export default connect(
  state => ({count: state}), // 这里的完整写法在上面基本使用中
  // mapDispatchToProps简写
  {
    increment:createIncrementAction,
    decrement:createDecrementAction,
    incrementAsync:createIncrementAsyncAction
  }
)(Count);
```



### connect方式，使用基本步骤

1. 定义UI组件—不暴露

2. 引入action

3. 使用connect创建出容器组件并暴露

   ```js
   // 基本写法
   export default connect(
   	state =>({key: value}), // 映射状态
     {
       key: xxxAction// 映射方法
     }
   )
   ```

4. 在UI组件中的props中会有对应的属性和方法，提供给我们操作

