# 组件实例属性props

作用：可以从组件外部传递数据进组件中



`props`的基本使用

```jsx
  <script type="text/babel">
    // 创建组件
    class Person extends React.Component {
      render() {
        console.log(this);
        const { name, age, sex } = this.props;
        return (
          <ul>
            <li>姓名:{name}</li>
            <li>性别:{sex}</li>
            <li>年龄:{age}</li>
          </ul>
        )
      }
    }
    const p = {name: '钱不二', age: 18, sex:'女'};
    // ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex} />, document.getElementById('test1'));
    
    // 简化版 babel+react 可以让我们使用...展开对象
    ReactDOM.render(<Person {...p} />, document.getElementById('test2'));
```



### 批量传递props

原理：**babel+react 可以让我们使用...展开对象，但是仅仅适用于标签属性的传递，其他位置用了没效果**，使用...

```jsx
    // ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex} />, document.getElementById('test1'));
    
    // 简化版 babel+react 可以让我们使用...展开对象
    ReactDOM.render(<Person {...p} />, document.getElementById('test2'));
```

