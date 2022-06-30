# 组件实例属性props

作用：**可以从组件外部传递数据进组件中**

也可以说是**标签属性**

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



### 批量传递props（批量传递标签属性）

原理：**babel+react 可以让我们使用...展开对象，但是仅仅适用于标签属性的传递，其他位置用了没效果**，使用...

```jsx
    // ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex} />, document.getElementById('test1'));
    const p = {name: '钱不二', age: 18, sex:'女'};
    // 简化版 babel+react 可以让我们使用...展开对象
    ReactDOM.render(<Person {...p} />, document.getElementById('test2'));
```

注意：在`jsx`中`{}`包裹的就是`js`表达式

### 对props进行类型限制

> 首先需要引入一个库`prop-types.js`，这样在全局就会多一个对象`PropTypes`（15.5版本之后就必须要引入这个库了）

注意：**注意这两个单词的大小写，有一点儿点儿区别**

>之后，对往组件上添加属性`propTypes`

```jsx
    // 创建组件
    class Person extends React.Component {
      // state = {name: 'tom', age:18, sex: "女"},
      render() {
        // console.log(this);
        const { name, age, sex } = this.props;
        // 多级结构最好包裹一下
        return (
          <ul>
            <li>姓名:{name}</li>
            <li>性别:{sex}</li>
            <li>年龄:{age+1}</li>
          </ul>
        )
      }
    }

    // 限制prop的类型
    Person.propTypes = {
      // 注意两个p的大小写不一样
      // isRequired为必传
      name: PropTypes.string.isRequired,
      sex: PropTypes.string,
      age: PropTypes.number,
      // 注意限制函数使用的是func
      speak: PropTypes.func
    }
    // 限制prop的默认值
    Person.defaultProps = {
      sex: '男女之间',
      age: 18
    }
    function speak(params) {
      console.log('我正在说话');
    }

    const p = {name: '钱不二', age: 18, sex:'女'};
    ReactDOM.render(<Person name="Jerry" age={19} speak={speak} />, document.getElementById('test1'));
    ReactDOM.render(<Person name='tom' age={20} sex='女' />, document.getElementById('test2'));
    // 简化版 jsx特有的方式
    ReactDOM.render(<Person {...p} />, document.getElementById('test3'));
```



### 对props默认值的限制

当没有对应的prop传递时，就会使用默认值

>和类型限制差不多，就是属性名变为`defaultProps`

```jsx
    // 限制prop的默认值
    Person.defaultProps = {
      sex: '男女之间', // 默认值
      age: 18
    }
```



### props限制的简写

首先，我们添加限制的方式，就是给`Person`组件类上添加属性，所以我们可以使用`static`关键字，直接在类中去添加属性

```jsx
// 创建组件
class Person extends React.Component {
  // 直接写进Person类中，不再写在外面了
  // 限制prop的类型
  static propTypes = {
    // 注意两个p的大小写不一样
    name: PropTypes.string.isRequired,
    sex: PropTypes.string,
    age: PropTypes.number,
    // 注意限制函数使用的是func
    speak: PropTypes.func
  }
  // 限制prop的默认值
  static defaultProps = {
    sex: '男女之间',
    age: 18
  }
  render() {
    // console.log(this);
    const { name, age, sex } = this.props;

    // this.props.name = 'jack'; // 此行代码报错，props是只读的

    // 多级结构最好包裹一下
    return (
      <ul>
        <li>姓名:{name}</li>
        <li>性别:{sex}</li>
        <li>年龄:{age+1}</li>
      </ul>
    )
  }
}
```



### 函数式组件使用props

函数式组件可以通过接收参数，来访问`props`

```jsx
function Person(props) {
  const {name, sex, age} = props;
  return (
      <ul>
        <li>姓名:{name}</li>
        <li>性别:{sex}</li>
        <li>年龄:{age+1}</li>
      </ul>
    )
}
Person.propTypes = {
  // 注意两个p的大小写不一样
  name: PropTypes.string.isRequired,
  sex: PropTypes.string,
  age: PropTypes.number,
  // 注意限制函数使用的是func
  speak: PropTypes.func
}
  // 限制prop的默认值
Person.defaultProps = {
  sex: '男女之间',
  age: 18
}

function speak(params) {
  console.log('我正在说话');
}
ReactDOM.render(<Person name="Jerry" sex='女' age={19} speak={speak} />, document.getElementById('test1'));
```





### props注意事项

第一：`props`是只读的（`vue`中也是只读的）

第二：如果写了构造器`constructor`必须要调用`super(props)`，否则会出现`props`未定义情况，（**构造器是否接收props，是否传递给super，取决于，是否想在实例中用this访问到props**）

```jsx
constructor(props) {
  // 接了还要传给super
	super(props)
}
```

