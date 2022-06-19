# React核心

## React组件绑定函数

React中给组件绑定函数和原生JS绑定函数基本一致，但是有一点儿区别

区别：要改成大驼峰写法 `onclick ---> onClick`,`onblur ---> onBlur`

```jsx
 return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
```



看代码：

```jsx
    // 1.创建组件
    class Weather extends React.Component {
      constructor(props) {
        // 有extends必须要super()一下
        super(props);

        this.state = {
          isHot: true
        }
      }
      render() {
        // console.log(this);
        const {isHot} = this.state;
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
      }

      changeWeather() {
        // changeWeather放在哪里？--Weather的原型上
        // 由于changeWeather作为onClick的回调，所以不是通过实例进行调用的，是直接调用
        // 然后类中的方法默认开启局部严格模式，所以changeWeather中的this为undefined
        this.state.isHot = false;
      }
    }
```

### 类中函数this指向问题

问题：为啥`changeWeather`中的`this`指向不是`Weather`的实例对象而是`undefined`?

核心原因是，`changeWeather`不是通过实例调用的。由于changeWeather作为onClick的回调，所以不是通过实例进行调用的，是直接调用，然后类中的方法默认开启局部严格模式，所以changeWeather中的this为undefined。

解决办法一：利用`bind`来在实例上添加一个新方法

```jsx
    class Weather extends React.Component {
      constructor(props) {
        // 有extends必须要super()一下
        super(props);

        this.state = {
          isHot: true
        }

        // 在实例上挂载一个新方法 新方法中的this指向Weather实例
        this.changeWeather = this.changeWeather.bind(this);
      }
      render() {
        // console.log(this);
        const {isHot} = this.state;
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
      }

      changeWeather() {
        console.log(this)
      }
    }
```

解决办法二：直接利用箭头函数

```jsx
  <script type="text/babel">

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





