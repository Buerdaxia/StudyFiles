# 组件实例属性refs

作用：组件内的标签，可以定义`ref`来标识自己，这里和`vue`中的一致

组件实例身上有一个属性叫做`refs`，会以键值对儿的方式收集`ref`

## 过时的refs，String类型Refs

这个版本不推荐使用了，因为有些问题（string类型的效率不高）

```jsx
  <script type="text/babel">
    // 创建组件
    class Demo extends React.Component {
      render(h) {
        return (
          <div>
            <input ref="input1" type="text" placeholder="点击按钮提示数据" />&nbsp;
            <button ref="button" onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
            <input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />
          </div>
        )
      }
      
      // 定义函数
      showData = () => {
        const {input1} = this.refs;
        alert(input1.value);
      }

      // 定义函数 展示右侧输入框数据
      showData2 = () => {
        const {input2} = this.refs;
        alert(input2.value);
      }
    }

    // 渲染组件到页面
    ReactDOM.render(<Demo />, document.getElementById('test'));
  </script>
```



## 常用的refs，回调函数型Refs

**可以在`ref`位置书写一个回调函数，回调函数的参数，就是当前`dom`节点。**

核心：书写一个回调函数，接收到参数，挂载到`this`,组件实例身上，使用时在取出

代码：

```jsx
  <script type="text/babel">
    // 创建组件
    class Demo extends React.Component {
      render(h) {
        return (
          <div>
            <input ref={(currentNode)=>{this.input1 = currentNode}} type="text" placeholder="点击按钮提示数据" />&nbsp;
            <button ref={c=>this.button = c} onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
            <input ref={c=>{this.input2 = c}} onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />
          </div>
        )
      }
      
      // 定义函数
      showData = () => {
        const {input1} = this;
        alert(input1.value);
      }

      // 定义函数 展示右侧输入框数据
      showData2 = () => {
        const {input2} = this;
        alert(input2.value);
      }
    }

    // 渲染组件到页面
    ReactDOM.render(<Demo />, document.getElementById('test'));
  </script>
```



### 回调函数型refs，函数调用小问题

官网：如果`ref`回调函数是以内联函数方式定义的，在更新过程中，回调函数会被执行两次，第一次传入参数为`null`，第二次传入`dom`元素

原因：就是每次渲染时创建一个新的函数实例，所以`React`为了清空旧的`ref`并设置新的，第一次会传递一个`null`来清空上次一的`dom`，第二次传递新`dom`

解决方式：写成class绑定的回调（不过这个小问题影响不大，几乎没影响）

类式绑定回调：

```jsx
  <script type="text/babel">
    // 创建组件
    class Demo extends React.Component {
      state = {isHot: true}

      render(h) {
        const {isHot} = this.state;
        return (
          <div>
          <h2>今天天气:{isHot?'炎热':'凉爽'}</h2>
            {/*<input ref={(c)=>{this.input1 = c;console.log('@',c)}} type="text" placeholder="点击按钮提示数据" />&nbsp;*/}
            {/*类式函数绑定*/}
            <input ref={this.saveInput} type="text" />
            <button ref={c=>this.button = c} onClick={this.showInfo}>点我提示左侧的数据</button>
            <button onClick={this.changeWeather}>点我切换天气</button>
          </div>
        )
      }
      // 上面类式ref函数绑定
      saveInput = (c) => {
        this.input1 = c;
        console.log('@', c);
      }

      changeWeather = () => {
        // 获取原来状态
        const {isHot} = this.state;
        
        // state状态更新
        this.setState({isHot: !isHot});
      }

      showInfo = ()=> {
        const {input1} = this;
        alert(input1.value);
      }
    }

    // 渲染组件到页面
    ReactDOM.render(<Demo />, document.getElementById('test'));
  </script>
```



## 比较新的refs，React.create()API

`React.create()`是`react`官方比较推荐的一种方式(也是最复杂的一种)，但是该`api`调用一次只能创建出一个专人专用的容器（只能存储一个`ref`）

容器`.current`获取存储的`ref`

代码：

```jsx
  <script src="../babel.min.js"></script>

  <!-- 注意书写的type一定要写babel，不写默认是text/javascript -->
  <script type="text/babel">
    // 创建组件
    class Demo extends React.Component {
      /*
        React.createRef调用后可以返回一个容器，用来存储ref标识的节点
        该容器是专人专用(只能存储一个)
      */
      myRef = React.createRef()
      myRef2 = React.createRef()
      render(h) {
        return (
          <div>
            <input ref={this.myRef} type="text" placeholder="点击按钮提示数据" />&nbsp;
            <button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
            <input ref={this.myRef2} onBlur={this.showData2} type="text" placeholder="点击按钮提示数据" />&nbsp;
          </div>
        )
      }
      
      // 定义函数
      showData = () => {
        console.log(this.myRef.current.value);
        alert(this.myRef.current.value);
      }

      // 失去焦点弹出
      showData = () => {
        alert(this.myRef2.current.value);
      }
    }

    // 渲染组件到页面
    ReactDOM.render(<Demo />, document.getElementById('test'));
  </script>
```

