# React中事件处理

## (1).通过onXxx属性指定事件处理函数(注意大小写)

1. aReact使用的是自定义(合成)事件，而不是使用原生DOM事件 ———— 为了更好的兼容性
2. React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ———— 为了高效



## (2).通过event.target（原生js也是这个获取发生事件源）得到发生事件的DOM元素对象 ———— 不要过度使用ref

示例：

```jsx
  <script type="text/babel">
    // 创建组件
    class Demo extends React.Component {

      /*
        (1).通过onXxx属性指定事件处理函数(注意大小写)
          a.React使用的是自定义(合成)事件，而不是使用原生DOM事件 ———— 为了更好的兼容性
          b.React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ———— 为了高效
        (2).通过event.target（原生js也是这个获取发生事件源）得到发生事件的DOM元素对象 ———— 不要过度使用ref
      */

      // 创建ref容器
      myRef = React.createRef()
      myRef2 = React.createRef()
      render(h) {
        return (
          <div>
            <input ref={this.myRef} type="text" placeholder="点击按钮提示数据" />&nbsp;
            <button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
            <input onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />&nbsp;
          </div>
        )
      }
      
      // 提示左侧输入框数据
      showData = () => {
        console.log(this.myRef.current.value);
        alert(this.myRef.current.value);
      }

      // 提示右侧输入框数据
      showData2 = (event) => {
        console.log(event)
        alert(event.target.value)
      }
    }

    // 渲染组件到页面
    ReactDOM.render(<Demo />, document.getElementById('test'));
  </script>
```

