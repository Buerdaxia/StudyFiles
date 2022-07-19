# `render props`与`props.children`

## 如何向组件内部动态传入带内容的结构(标签)?

	Vue中: 
		使用slot技术, 也就是通过组件标签体传入结构  <A><B/></A>
	React中:
		使用children props: 通过组件标签体传入结构
		使用render props: 通过组件标签属性传入结构,而且可以携带数据，一般用render函数属性

### children props

	<A>
	  <B>xxxx</B>
	</A>
	{this.props.children}
	问题: 如果B组件需要A组件内的数据, ==> 做不到 

### render props

	<A render={(data) => <C data={data}></C>}></A>
	A组件: {this.props.render(内部state数据)}
	C组件: 读取A组件传入的数据显示 {this.props.data} 

代码示例：

```jsx
import React, { Component } from 'react'
import './index.css';
export default class Parent extends Component {
  render() {
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        {/* 给B传递一个render函数，返回B组件，并且还能传递形参 */}
        <A render={(name) => <B name={name}></B>} />
      </div>
    )
  }
}

class A extends Component {
  state = {name :'tom'}
  render() {
    console.log(this);
    const {name} = this.state;
    return (
      <div className='a'>
        <h3>我是A组件</h3>
        {/* <h4>{this.props.children}</h4> */}
        {/* <B></B> */}
        {/* 这里调用传来的render函数,并且将name传递过去 */}
        {this.props.render(name)}
      </div>
    )
  }
}

class B extends Component {
  render() {
    return (
      <div className='b'>
        <h3>我是B组件</h3>
        <h4>我收到的名称: {this.props.name}</h4>
      </div>
    )
  }
}
```

