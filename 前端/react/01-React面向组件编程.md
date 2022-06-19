# React面向组件编程

组件分为两种：函数式组件和类式组件

## 函数式组件

函数式组件的写法：首先函数函数，肯定和函数有关。

**函数名儿，就是组件名**

>第一步：创建一个函数式组件

```jsx
  //  1.创建函数式组件
  function MyComponent() {
    console.log(this)// 此处的this是undefined，应为babel翻译后会开启严格模式
    return <h2>我是用函数定义的组件（适用于简单的情况）</h2>
  }
	/*
		函数名要大写
	*/
```

>第二步：渲染组件到页面上

```jsx
  // 2.渲染组件到页面上
	// 注意参数一的位置上：直接写组件标签
  ReactDOM.render(<MyComponent/>,document.getElementById('test'));
```



执行了ReactDOM.render(<MyComponent/>,document.getElementById('test'));之后发生了什么？

1. React解析组件标签，找到了MyComponent组件。

2. React发现组件是用函数定义的，随后就调用该函数，将返回的虚拟DOM转换为真实DOM

​    呈现在页面上



## 类式组件

类式组件就和类的创建一致，不过要继承一个React自带的父类`React.Component`

```jsx
  <!-- 注意书写的type一定要写babel，不写默认是text/javascript -->
  <script type="text/babel">
  //  1.创建类式组件
  // 使用类式组件，必须要继承一个React的类
  class MyComponent extends React.Component{
    render() {
      // render是放在哪里的？--放在MyComponent的原型对象上，供实例使用
      // render中的this是谁？--MyComponent的实例对象（MyComponent组件实例对象）
      return <h2>我是用类定义的组件（适用于复杂的情况）</h2>
    }
  }

  // 2.渲染组件到页面
  ReactDOM.render(<MyComponent/>, document.getElementById('test'))

    /*
    执行了ReactDOM.render(<MyComponent/>,document.getElementById('test'));
    之后发送了什么？

    1. React解析组件标签，找到了MyComponent组件。
    2. React发现组件是用类定义的，随后new出来该类的实例，并通过该实例调用到render()方法
    3. 将render中返回的虚拟DOM转换为真实DOM并渲染在页面上
  */
  </script>
```

