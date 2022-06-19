# jsx

全称: JavaScript XML

本质：react定义的一种类似于XML的JS扩展语法: JS + XML本质是**React.createElement(component, props, ...children)**方法的语法糖

作用：用来简化语法



注意:它不是字符串, 也不是HTML/XML标签

## jsx的语法规则

1. 定义虚拟DOM时，不要写引号

   ```jsx
   // 1.创建虚拟DOM
       const VDOM = (
         <h2 id='ambition'>
           <span>hello react</span>
         </h2>)
       // 没有引号
   ```

   

> 第二：标签中混入`JS`的表达式时，要使用`{}`
>
> 注意：主语区分什么是`js`表达式和`js`语句

表达式：表达式会产生一个值，可以放在任意一个需要值的地方

例如：

1. a
2. a+b
3. demo(1)
4. arr.map()
5. function test() {}

特点：以上的表达式，都可以在左侧定义一个值来接收

语句：

例如：

1. if()
2. for(){}
3. switch() {case:...}

```jsx
    const myId = 'ambition'
    const myData = 'Hello, React'
    // 1.创建虚拟DOM
    const VDOM = (
      <h2 id={myId}>
        <span>{myData}</span>
      </h2>)
```



>第三：标签中，样式的类名不能写class，要写className

```js
    const VDOM = (
      <h2 className="title" id={myId}>
        <span>{myData}</span>
      </h2>)
```





>第四：内联样式，要用style={{key:value}}方式写
>
>注意：key还必须是小驼峰写法，例如font-size写成fontSize

```jsx
    // 1.创建虚拟DOM
    const VDOM = (
      <h2 className="title" id={myId}>
        <span style={{color:'white', fontSize:'30px'}}>{myData}</span>
      </h2>)
```



>第五：书写标签只能有一个根标签

```js
    const VDOM = (
      <div> // 这里div就是根标签
          <h2 className="title" id={myId}>
            <span style={{color:'white', fontSize:'30px'}}>{myData}</span>
          </h2>
          <h2 className="title" id={myId.toUpperCase()}>
            <span style={{color:'white', fontSize:'30px'}}>{myData}</span>
          </h2>
      </div>
      )
```



>第六：所有标签必须都要闭合



>第七：标签首字母区别

1. 如果小写字母开头，则转换标签为html中同名的元素，若html没有该标签则报错

```jsx
<div></div> 会直接转换为html的div

<good></good> 会直接报错，因为html中暂时没good标签
```



2. 如果标签是大写字母开头，React会去渲染对应的组件，若组件没有定义，则报错

```jsx
<Good></Good>会直接被当作为组件component
```

