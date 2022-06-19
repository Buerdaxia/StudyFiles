# 组件实例对象属性state

state是组件对象中最重要的属性，值是对象（可以包含多个key-value）的组合。

组件又被称为“状态机”，通过更新组件的`state`来更新对应的页面（重新渲染组件）



1. `state`属性在组件实例对象身上
2. `state`属性为一个对象
3. `state`通过`constructor`进行初始化
4. 严重注意：不能直接修改`state`，需要内置`API`使用`setState()`修改



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
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'},{wind}</h1>
      }

      // changeWeather调用几次？ --- 触发几次调几次
      changeWeather() {
        // changeWeather放在哪里？--Weather的原型上
        // 由于changeWeather作为onClick的回调，所以不是通过实例进行调用的，是直接调用
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

