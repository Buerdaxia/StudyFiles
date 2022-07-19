# 组件实例对象属性state

state是组件对象中最重要的属性，值是对象（可以包含多个key-value）的组合。

组件又被称为“状态机”，通过更新组件的`state`来更新对应的页面（重新渲染组件）



1. `state`属性在组件实例对象身上
2. `state`属性为一个对象
3. `state`通过`constructor`进行初始化
4. 严重注意：**不能直接修改`state`，需要内置`API`使用`setState()`修改**



### **state标准版( •̀ ω •́ )y**

```jsx
  <!-- 注意书写的type一定要写babel，不写默认是text/javascript -->
  <script type="text/babel">

    // 1.创建组件
    class Weather extends React.Component {
      // 构造器调用几次？ --- 1次
      constructor(props) {
        // 有extends必须要super()一下
        super(props);

        this.state = {
          isHot: true,
          wind: '和风'
        }

        // 在实例上挂载一个新方法
        this.changeWeather = this.changeWeather.bind(this);
      }

      // render()函数调用几次？ --- 1+n次，n为state修改的次数
      render() {
        // console.log(this);
        const {isHot,wind} = this.state;
        // this.changeWeather没有调用，是点击之后再调用
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'},{wind}</h1>
      }

      // changeWeather调用几次？ --- 触发几次调几次
      changeWeather() {
        // changeWeather放在哪里？--Weather的原型上
        // 由于changeWeather作为onClick的回调（触发后才调用），所以不是通过实例进行调用的，是直接调用
        // 然后类中的方法默认开启局部严格模式，所以changeWeather中的this为undefined
        const {isHot} = this.state;
        // 严重注意：状态(state)不可以直接更改，需要借助一个内置API
        // 以下为直接更改，无效！！！
        // this.state.isHot = !isHot;



        // 严重注意：状态必须通过setState进行更改，且更新是一种合并，不是替换
        this.setState({isHot: !isHot});
      }
    }

    ReactDOM.render(<Weather/>, document.getElementById('test'));
    
  </script>
```



### **简化版(●'◡'●)**

```jsx
    // 1.创建组件
    class Weather extends React.Component {
      // 初始化状态
      state = {
        isHot: true,
        wind: '和风'
      }
      
      render() {
        const {isHot,wind} = this.state;
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'},{wind}</h1>
      }

      // 自定义方法 --- 要用赋值语句的形式+箭头函数
      changeWeather = ()=>{
        const {isHot} = this.state;
        this.setState({isHot: !isHot});
      }
    }

    ReactDOM.render(<Weather/>, document.getElementById('test'));
    
  </script>
```



## state相关API

### `setState`

`setState()` 将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。这是用于更新用户界面以响应事件处理器和处理服务器数据的主要方式

注意：**React更新状态的动作是异步的**(简单来说，就是setState调用后修改属性的操作是异步的)

`setState`更新状态的2种方法：

```jsx
	(1). setState(stateChange, [callback])------对象式的setState
            1.stateChange为状态改变对象(该对象可以体现出状态的更改)
            2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
					
	(2). setState(updater, [callback])------函数式的setState
            1.updater为返回stateChange对象的函数。
            2.updater可以接收到state和props。
            4.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。
总结:
		1.对象式的setState是函数式的setState的简写方式(语法糖)
		2.使用原则：
				(1).如果新状态不依赖于原状态 ===> 使用对象方式
				(2).如果新状态依赖于原状态 ===> 使用函数方式
				(3).如果需要在setState()执行后获取最新的状态数据, 
					要在第二个callback函数中读取

示例：
  add = () => {
    const {count} = this.state;
    // 对象式的setState
    // this.setState({count: count+1}, () => {
    //   console.log('setState回调中的count:',this.state.count);
    // });
    // // 此时count还未更新完毕
    // console.log('现在的count:' ,this.state.count);
  
    // 函数式的setState
    this.setState((state, props)=> {
      console.log(state, props);
      return {count: state.count+1};
    })

  }
```



### `useState`（Hooks）

返回一个 state，以及更新 state 的函数。

语法:

```jsx

const [state, setState] = React.useState(initialState);
// state：是接收状态的变量第一个指是initialState
// setState是改变状态的方法
// 数组的解构赋值
```



示例：使用`useState`

```jsx
import { useState } from 'react';
function Button(props) {
	const [color, setColor] = useState('red');
	const handleButtonClick = () => {
		setColor('green');
	};
	const { children } = props;
	return (
		<div>
			<button onClick={handleButtonClick}>{children}</button>
			<span style={{ color }}>这时一段文字</span>
		</div>
	);
}

export default Button;

```

