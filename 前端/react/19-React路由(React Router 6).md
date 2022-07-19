# React路由(React Router 6)

这里任然讲述`react-router-dom`针对web开发。



相比于`React Router 5.x`，主要是修改了以下三大类：

1. 内置组件的变化：移除`<Switch />`新增`<Routes/>`
2. 语法的变化`component={About}`改编为`element={<About />}`
3. 新增多个hook：`useParams`、`useNavigate`、`useMatch`等



## Component

### `<Routes>`

<Routes>作为Route标签的外标签。

作用：当URL发生变化时，`<Routes>`会查看其所有子`<Route>`元素并找到最佳匹配并呈现！。

示例：

```jsx
import React from 'react';
import { NavLink, Route, Routes, Navigate } from 'react-router-dom';

import About from './pages/About';
import Home from './pages/Home';

function App(props) {
	return (
		<div>
			<div className="row">
				<div className="col-xs-offset-2 col-xs-8">
					<div className="page-header">
						<h2>React Router Demo</h2>
					</div>
				</div>
			</div>
			<div className="row">
				<div className="col-xs-2 col-xs-offset-2">
					<div className="list-group">
						{/* 路由链接 */}
						<NavLink className="list-group-item" to="/about">
							About
						</NavLink>
						<NavLink className="list-group-item" to="/home">
							Home
						</NavLink>
					</div>
				</div>
				<div className="col-xs-6">
					<div className="panel">
						<div className="panel-body">
							{/* 注册路由, Switch没了,多了一个Routes */}
							<Routes>
								<Route path="/about" element={<About></About>}></Route>
								<Route path="/home" element={<Home></Home>}></Route>
								<Route path="/" element={<Navigate to="/about"></Navigate>}></Route>
							</Routes>
						</div>
					</div>
				</div>
			</div>
		</div>
	);
}

export default App;

```





### `<Route>`

`Route`标签的功能和5里一致，就是语法变了一丢丢：

```js
	// 5
<Route path="/about" component={组件名儿} />

  // 6
<Route path="/about" element={<组件名儿 />} />
```



并且：Route并且必须要配合Routes使用，必须要用`<Routes>`包裹`<Route>`

高级使用：`<Route>`可以配合`useRoutes`配置路由表，但是需要配合`<Outlet>`标签使用



### `<Navigate>`

作用：只要`<Navigate>`组件被渲染，就会修改路径，并且切换视图。

属性：

1. to 代表路径
2. replace可以将跳转模式转变为replace模式，默认是push

可以进行路由的重定向，可以用来替代5中的<Redirect />

重定向：

```jsx
<Routes>
  <Route path="/about" element={<About></About>}></Route>
  <Route path="/home" element={<Home></Home>}></Route>
  {/*重定向到/about*/}
  <Route path="/" element={<Navigate to="/about"></Navigate>}></Route>
</Routes>
```



示例：

```jsx
import React, {useState} from 'react'
import {Navigate} from 'react-router-dom'
export default function Home() {
  const [sum, setSum] = useState(0);

  return (
    <div>
      <div>Home</div>
      {sum === 2 ? <Navigate to="/about" replace></Navigate> : <h2>sum的值为:{sum}</h2>}
      <button onClick={() => setSum(2)}>点我将sum变为2</button>
    </div>
  )
}

```



### `<NavLink>`

NavLink的作用和5中的一致，有变化的是激活样式的设置。

属性：

1. replace 路由跳转模式
2. end 表示只展示子标签激活属性，父路由激活样式不显示
3. className 样式与激活样式
4. to 跳转path

激活样式设置现在要这样写：

```jsx
// 定义一个激活类函数
function computedClassName({ isActive }) {
  return isActive ? 'list-group-item s' : 'list-group-item';
}


<div className="list-group">
  {/* s 是高亮样式 */}
  <NavLink
    className={({ isActive }) => (isActive ? 'list-group-item s' : 'list-group-item')}
    to="/about"
  >
    About
  </NavLink>
  <NavLink className={computedClassName} to="/home">
    Home
  </NavLink>
</div>
```

语法：

`className`属性要写成一个回调函数，函数形参结构出`isActive` ，如果选中了`isActive`会变成`true`。





### `<Outlet>`

作用：呈现匹配到路由组件和vue中的`<router-view>`标签特别像，功能上基本一致。

**经常配合路由表使用**。

示例：

```jsx
import React from 'react'
import {NavLink, Outlet} from 'react-router-dom'
export default function Home() {
 

  return (
    <div>
      <h2>Home组件内容</h2>
      <div>
        <ul className="nav nav-tabs">
          <li>
            <NavLink className="list-group-item" to="news">News</NavLink>
          </li>
          <li>
            <NavLink className="list-group-item" to="message">Message</NavLink>
          </li>
        </ul>
        <div>
          {/*展示匹配到的路由组件*/}
          <Outlet></Outlet>
        </div>
      </div>
    </div>
  )
}

```



## API

### `useRoutes` 路由表

可以通过`useRoutes`函数创建一个路由表，用来注册路由。

语法：

```js
import { useRoutes } from 'react-router-dom';
import Home from '..xxx'
const element = useRoutes([
  {
    path: '/home',
    element: <Home />
  }
])
  
  
  // 然后在注册路由的时候直接写
  {element}
```



一般我们可以自己创建一个`routes`来存储路由，使用时再引入回来：

示例：

创建：`routes/index.js`

```js
import { Navigate } from 'react-router-dom';
import Home from '../pages/Home';
import About from '../pages/About';
let routes = [
	{
		path: '/about',
		element: <About></About>
	},
	{
		path: '/home',
		element: <Home></Home>
	},
	{
		path: '/',
		element: <Navigate to="/about"></Navigate>
	}
];

export default routes;

```



在`App.js`中引入：

```jsx
import React from 'react';
import { NavLink, useRoutes } from 'react-router-dom';
// 引入路由表参数
import routes from './routes';

function App(props) {
	// 书写一个路由表
	const element = useRoutes(routes);

	return (
		<div>
			<div className="row">
				<div className="col-xs-offset-2 col-xs-8">
					<div className="page-header">
						<h2>React Router Demo</h2>
					</div>
				</div>
			</div>
			<div className="row">
				<div className="col-xs-2 col-xs-offset-2">
					<div className="list-group">
						{/* 路由链接 */}
						<NavLink className="list-group-item" to="/about">
							About
						</NavLink>
						<NavLink className="list-group-item" to="/home">
							Home
						</NavLink>
					</div>
				</div>
				<div className="col-xs-6">
					<div className="panel">
						<div className="panel-body">
							{/*通过路由表 注册路由 */}
							{element}
						</div>
					</div>
				</div>
			</div>
		</div>
	);
}

export default App;

```



#### 使用路由表的嵌套路由

首先看`routes/index.js`：

嵌套路由的写法，不能说和vue一致吧，只能说是一模一样了！

```js
import { Navigate } from 'react-router-dom';
import Home from '../pages/Home';
import About from '../pages/About';
import Message from '../pages/Message';
import News from '../pages/News';
let routes = [
	{
		path: '/about',
		element: <About></About>
	},
	{
		path: '/home',
		element: <Home></Home>,
		children: [
			{
				path: 'news',
				element: <News></News>
			},
			{
				path: 'message',
				element: <Message></Message>
			}
		]
	},
	{
		path: '/',
		element: <Navigate to="/about"></Navigate>
	}
];

export default routes;

```



之后展示路由的位置使用`<Outlet>`组件：

```jsx
import React from 'react'
import {NavLink, Outlet} from 'react-router-dom'
export default function Home() {
 

  return (
    <div>
      <h2>Home组件内容</h2>
      <div>
        <ul className="nav nav-tabs">
          <li>
            <NavLink className="list-group-item" to="news">News</NavLink>
          </li>
          <li>
            <NavLink className="list-group-item" to="message">Message</NavLink>
          </li>
        </ul>
        <div>
          <Outlet></Outlet>
        </div>
      </div>
    </div>
  )
}

```





### `useParams`

作用：调用后，返回路由组件接收到的`params`参数

示例：

```jsx
import React from 'react'
import { useParams } from 'react-router-dom'
export default function Detail() {
  const { id, title, content } = useParams();
  // console.log(p);
  return (
    <div>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </div>
  )
}

```



### `useSearchParams`

用来获取和search参数有关的数组，和`useState`特别像

返回结果：是一个数组

示例：

```jsx
import React from 'react'
import {useSearchParams} from 'react-router-dom'
export default function Detail() {

  const [search, setSearch] = useSearchParams();
  // 要调用get方法
  const id = search.get('id');
  const title = search.get('title');
  const content = search.get('content');
  return (
    <div>
      <li>
        <button onClick={() => setSearch('id=004&title=嘻嘻&content=赣深麽')} >点我更新Search</button>
      </li>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </div>
  )
}

```



### `useMatch`

该方法可以获取router5中的路由组件身上携带的Match属性

```jsx
import {useMatch} from 'react-router-dom'
const match = useMatch();
```



### `useLocation`

该方法可以获取和router 5 中路由组件身上的Location属性：

```jsx
import {useLocation} from 'react-router-dom'
const location = useLocation();
```



### `useNavigate`(编程导航)

用于创建一个实例，和编程时路由导航息息相关，看下面的编程时路由导航。



### 不常用：`useInRouterContext`

作用：如果组件在`<Router>`的上下文中呈现，则`useInRouterContext`钩子返回`true`，否则返回`false`



就是可以检查检查，自己(或别人)的组件是否被`<BrowserRouter>`或者`<HushRouter>`包裹



### 不常用：`useNavigationType`

作用：返回当前的导航类型（用户是如何来到当前页面的）。

返回值：`POP`、`PUSH`、`REPLACE`

备注：`POP`是指在浏览器中直接打开了这个路由组件（刷新页面） 



### 不常用：`useOutlet`

作用：用来呈现当前组件中渲染的嵌套路由

实例：

```jsx
const result = useOutlet();
console.log(result);
/*
	如果嵌套路由没有挂载，则result为null
	如果嵌套路由依据挂载，则展示嵌套路由对象
*/
```



### 不常用：`useResolvedPath`

作用：给定一个URL值，解析其中的：path、search、hash值





## 传递参数

6和5有那么一点点儿的区别。

### `params`参数

首先：传递参数和接收参数和router 5一样

传递参数：

```jsx
<Link to={`detail/${m.id}/${m.title}/${m.content}`}>{m.title}</Link>&nbsp;&nbsp;
```



接收参数：

```js
// 有一点儿点儿区别的是，这里是在路由表写参数接收
{
  path: 'message',
  element: <Message></Message>,
  children: [
    {
      path: 'detail/:id/:title/:content',
      element: <Detail></Detail>
    }
  ]
}
```



使用参数：

使用参数需要一个`hooks`——>`useParams`：

```jsx
import React from 'react'
import { useParams } from 'react-router-dom'
export default function Detail() {
  const { id, title, content } = useParams();
  // console.log(p);
  return (
    <div>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </div>
  )
}

```





### `search`参数

首先：传递参数和router 5 一样

传递参数：

```jsx
<Link to={`detail?id=${m.id}&title=${m.title}&content=${m.content}`}>{m.title}</Link>
```



使用参数：

```jsx
import React from 'react'
import {useSearchParams} from 'react-router-dom'
export default function Detail() {

  const [search, setSearch] = useSearchParams();
  // 要调用get方法
  const id = search.get('id');
  const title = search.get('title');
  const content = search.get('content');
  return (
    <div>
      <li>
        {/*setSearch传入一个`urlencoded`编码的字符串。*/}
        <button onClick={() => setSearch('id=004&title=嘻嘻&content=赣深麽')} >点我更新Search</button>
      </li>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </div>
  )
}
```



### `state`参数

这个改动多一点儿：

首先传递参数：直接写一个`state`标签属性

```jsx
<Link 
  to="detail" 
  state={{
    id: m.id,
    title: m.title,
    content: m.content
  }} 
>{m.title}</Link>&nbsp;&nbsp;
```



接收并使用：

```jsx
import React from 'react'
import {useLocation} from 'react-router-dom'
export default function Detail() {
  // 连续结构赋值
  const {state:{id, title, content}} = useLocation();
  
  return (
    <div>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </div>
  )
}
```





## 编程时路由导航

router 6的编程时路由导航和`useNavigate`函数紧密相关：

直接上代码：编程时路由导航进行路由跳转

```jsx
import React, {useState} from 'react'
// 1.引入useNavigate
import { Outlet, Link, useNavigate } from 'react-router-dom'
export default function Message() {
  // 2.创建
  const navigate = useNavigate();
  const [messages] = useState([
    {id: '001', title: '消息1', content: '我是消息1'},
    {id: '002', title: '消息2', content: '我是消息2'},
    {id: '003', title: '消息3', content: '我是消息3'},
    {id: '004', title: '消息4', content: '我是消息4'}
  ])


  const show = (m) => {
    // 3.跳转并传递参数
    navigate('detail', {
      replace: false,
      state: {
        id: m.id,
        title: m.title,
        content: m.content
      }
    })
  }


  return (
    <div>
      <ul>
        {messages.map((m)=>{
          return (
            <li key={m.id} >
              <Link 
                to="detail" 
                state={{
                  id: m.id,
                  title: m.title,
                  content: m.content
                }} 
              >{m.title}</Link>
              {/*4.点击并跳转*/}
              <button onClick={()=>show(m)} >点我查看详情</button>
            </li>
          )
        })}
      </ul>
        <hr />
      <div>
        <Outlet></Outlet>
      </div>
    </div>
  )
}

```



前进和后退：

```jsx
const navigate = useNavigate();
function forward() {
  // 直接传递
  navigate(1);
}

function back() {
  navigate(-1)
}
```



