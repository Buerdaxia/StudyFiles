# React中的生命周期(究极无敌重要)

定义：组件从创建到死亡会经历一些特定的阶段。`React`组件中包含一系列钩子函数，会在特定的时刻进行调用。我们定义组件时，会在特定的生命周期回调函数中，做特定的工作。



## 生命周期流程图(旧)

![1-react旧版生命周期](../../前端图片/react/1-react旧版生命周期.png)

注意（这有自己的特点）：

`componentWillReceiveProps()`：是接收新的`Props`时才会调用，第一次不会调用

`shouldComponentUpdate()`：是控制组件是否更新的阀门，需要传递一个`Boolean`值，默认(不写的时候)情况传递`true`

### 三个阶段（旧）

**初始化阶段:** 由`ReactDOM.render()`触发---初次渲染

1. `constructor()`

2. `componentWillMount()`

3. `render()` ====>常用

4. `componentDidMount() ` ====>常用

   一般在这个钩子中做初始化工作，例如开启定时器、发起网络请求、发起订阅

 **更新阶段:** 由组件内部`this.setSate()`或父组件重新render触发

1. `shouldComponentUpdate()`

2. `componentWillUpdate()`

3. `render()`

4. `componentDidUpdate()`

**卸载组件:** 由`ReactDOM.unmountComponentAtNode()`触发

1. `componentWillUnmount() ` ====>常用

   一般在这个狗子中做收尾工作，如：取消定时器、关闭订阅



## 新版本和旧版本区别

新版本”废除“了三个生命周期钩子`componentWillMount`、`componentWillReceiveProps`、`componentWillUpdate`

新添加了两个生命周期钩子`getDerivedStateFromProps`、`getSnapshotBeforeUpdate`(这俩钩子的使用频率及其**罕见**)

## 生命周期流程图(新)

![2-react生命周期(新)](../../前端图片/react/2-react生命周期(新).png)

新东西：**罕见**

`getDerivedStateFromProps(props, state)`：此方法适用于[罕见的用例](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state)，即 state 的值在任何时候都取决于 props。

`getSnapshotBeforeUpdate(prevProps, prevState, snapshot)`：在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 `componentDidUpdate()`。

### 三个阶段（新）

 **初始化阶段:** 由`ReactDOM.render()`触发---初次渲染

1. `constructor()`

2. **`getDerivedStateFromProps()`** 

3. <strong style="color: red">`render()` ===>常用</strong>

4. <strong style="color: red">`componentDidMount()` ===>常用</strong>

**更新阶段:** 由组件内部`this.setSate()`或父组件重新render触发

1. **`getDerivedStateFromProps`**

2. `shouldComponentUpdate()`阀门儿

3. `render()`

4. **`getSnapshotBeforeUpdate`**

5. `componentDidUpdate()`

**卸载组件:** 由`ReactDOM.unmountComponentAtNode()`触发

1. <strong style="color: red">`componentWillUnmount()` ===>常用</strong>



## 三个重要的钩子

1. `render`：初始化渲染或者更新渲染
2. `componentDidMount`：开启监听、发送`ajax`
3. `componentWillUnmount`：做一些收尾工作





## 即将要废除的三个钩子

1. `componentWillMount`
2. `componentWillUpdate`
3. `componentWillReceiveProps`