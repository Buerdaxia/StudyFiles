# 组件间通讯方式Context

作用：一种组件间通讯方式，常用于**【组组件】与【后代组件】**间的通信。

`context`属性就放在组件自生实例上！



## 基本使用

>第一步：创建一个Context

```jsx
// 创建一个用于保存用户名儿的Context
const UserNameContext = React.createContext();
```



>第二步：渲染子组件时，外面要包裹`XxxContext.Provider`,并且通过`value`属性给后代组件传递数据：

```jsx
{/* 注意value名字不能修改 */}
<UserNameContext.Provider value={username}>
  <B></B>
</UserNameContext.Provider>
```



>第三步：（类式组件）要接收的数据的后代组件需要举手声明🙋‍说：我我我我要用！
>
>通过：`static contextType = xxxContext;`

```jsx
// 如果C是类式组件
class C extends Component {
  // 调用一下contextType说一声，我要用context然后就能接受啦
  static contextType = UserNameContext;
  render() {
    console.log(this);
    return (
      <div className="s">
        <h2>我是C组件</h2>
        <h4>我从A组件接收到的用户名儿是:{this.context}</h4>
      </div>
    )
  }
}
```



>第三步2：（函数式组件）函数式组件需要在使用数据的位置用`xxxContext.Consumer`标签包裹使用

```jsx
// 如C是函数式组件
function C() {
  return (
    <div className="s">
      <h2>我是C组件</h2>
      <h4>我从A组件接收到的用户名儿是:
        {/* 函数式组件要使用该标签 */}
        <UserNameContext.Consumer>
          {
            (value) => {
              return value;
            }
          }
        </UserNameContext.Consumer>
      </h4>
    </div>
  )
}
```

