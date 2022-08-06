# Vue中的路由（一个插件库）

vue的一个插件儿库，专门用来实现SPA应用。



## 啥事SPA

1. 单WEB应用(single page web application, SPA).
2. 整个应用只有一个完整页面
3. 点击页面中的导航链接不会刷新页面，只会做页面的局部更新
4. 数据需要通过ajax请求获取



## 路由的理解

### 什么事路由？

1. 一个路由就是一组映射关系（key - value）
2. key为路径，value可能是function或component



### 路由的分类

1）后端路由：

1. value是function，用于请求客户端提交的请求。
2. 工作过程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据。

2）前端路由：

1. value是component，用于展示页面内容。
2. 工作过程：当浏览器的路径改变时，对应的组件就会显示。





# 路由的基本使用

1）安装vue-router，命令：`npm i vue-router`

2）应用插件：在入口文件main.js中`Vue.use(VueRouter)`，这个也可以放置到路由的配置文件里，不是必须写在main.js上

```js
import VueRouter from 'vue-router';
Vue.use(VueRouter);

然后引入写好的router文件
import router from '../router/xxxx';

//然后添加进配置项即可
new Vue({
    // 指定绑定容器
    el: '#app',
    // 这是指定放入容器的组件
    render: h => h(App),
    // router: router,
    // 简写
    router
})
```

3）编写router配置项：

```javascript
//引入VueRouter
import VueRouter from 'vue-router';

//引入需要使用的组件
import About from '../components/About';
import Home  from '../components/Home';

//创建router实例对象，去管理一组一组的路由规则
const router = new VueRouter({
    routes: [
        {
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home
        },
    ]
})

//将router暴露出去
export default router;


//创建并暴露de写法
export default new VueRouter({
    routes: [
        {
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home
        },
    ]
})
```

4）实现切换（active-class可以配置高亮样式）

active-class属性：是那个被激活了就加上后续属性

`active-class="active"`

```html
<!-- vue中借助router-link实现路由的切换 -->
<router-link class="list-group-item" active-class="active" to="/about">About</router-link>

<router-link class="list-group-item" active-class="active" to="/home" >Home</router-link>
```

5）指定展示组件的位置，（和插槽slot很像）

```html
<!-- 指定组件的呈现位置 -->
<router-view></router-view>
```



## router-view与router-link

`<router-view></router-view>`标签用来展示路由组件呈现的位置。一般都是写在`App.vue`根组件下(为了展示路由组件)。

---

`<router-link to="跳转位置"></router-link>`标签，实质上和`<a></a>`标签很像，点击之后跳转到to下面指定的路由。

`<router-link to="跳转位置"></router-link>`一般写在各个组件中，和a标签写法一致



## 几个注意点

1）路由组件通常存放在`pages`文件夹，一般组件通常存放在`components`文件夹下。

2）通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。

3）每个组件都有自己的`$route`属性，里面存储着自己的路由信息。

4）整个应用只有一个router，可以通过组件的`$router`属性获取到。

## $route与$router

每个组件身上都有一个`$route`和`$router`属性，`$route`里面存储着自己的路由信息。

而`$router`属性一般在路由组件上使用，可以进行编程式路由跳转（看下面的编程式路由导航）

```js
//然后添加进配置项即可
new Vue({
    // 指定绑定容器
    el: '#app',
    // 这是指定放入容器的组件
    render: h => h(App),
    // router: router,
    // 简写
    router
})
```



## 多级（嵌套）路由

1）配置路由规则，使用children配置项：

```javascript
// 该文件专门用于创建整个应用的路由器
// 引入router
import VueRouter from 'vue-router';

// 引入组件
import About from '../pages/About';
import Home  from '../pages/Home';
import News from '../pages/News';
import Message from '../pages/Message';
// 创建一个路由器并暴露
export default new VueRouter({
    routes: [
        {
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home,
            // 子路由
            children:[
                {
                    //注意此处没/
                    path: 'news',
                    component: News
                },
                {
                    path: 'message',
                    component: Message
                },                
            ]
        },
    ]
})
```

2）跳转（要写完整路径）

```html
<router-link to="/home/news">News</router-link>
```



## 路由的query参数

**编程式路由导航的写法和这个一致**

1）传递参数

```html
<!-- 跳转路由携带query参数，to的字符串写法 -->
<router-link 
:to="`/home/message/detail?id=${m.id}&title=${m.title}`"
>
{{m.title}}
</router-link>


<!-- 跳转路由携带query参数，to的对象写法 -->
<router-link 
:to="{
    path: '/home/message/detail',
    query: {
        id: m.id,
        title: m.title
    }
}"
>
{{m.title}}
</router-link>
```

2）接收参数：

```javascript
//上面发的参数都发到了$route下的query中
$route.query.id
$route.query.title
```

**编程式路由写法**

query参数需要加上?（问号，然后以key=value方式传递）

```js
//字符串写法 加上? 
this.$router.push(`/home/message/detail?id=${m.id}&title=${m.title}`)

//对象写法
this.$router.push({
    path: '/home/message/detail',
    query: {
        id: m.id,
        title: m.title
    }
})
```







## 命名路由

1）作用：可以简化路由的跳转，并且可以相同如果有相同path，通过name也能进行跳转，可以避免有歧义的URL

2）如何使用

首先给路由命名：

```javascript
{
	path: 'demo',
	component: Demo,
	children:[
		{
			path: 'test',
			component: Test,
			children:[
				name: 'hello',
				path: 'welcome',
				component: Hello
			]
		}
	]
}
```

然后就可以简化跳转了：

```html
<!-- 简化前完整跳转 -->
<router-link to="/demo/test/welcome">跳转</router-link>
//简化后 直接通过名字跳转
<router-link :to="{name: hello}">跳转</router-link>

<!-- 简化写法配合参数 -->
<router-link
:to="{
	name:'hello',
	query: {
		id:666,
		title:'你好'
	}
}"
</router-link>       

<!--如果要传递params参数-->
<router-link
:to="{
	name:'hello',
	params: {
		id:666,
		title:'你好'
	}
}"
</router-link>   
```



## 路由别名alias

路由还支持给路由起别名，这样对应的路由组件相当于有两个路径了，访问这两个路径都能跳转。

示例：

```js
{
	path: '/demo',
	component: Demo,
	alias: '/posts'
}

// 假如携带参数，alias中也要携带
{
	path: '/:UserId',
	component: User,
	alias: '/user/:UserId'
}


// 设置多个别名，用数组形式书写,现在User有三个URL了
{
	path: '/',
	component: User,
	alias: ['/user', '/people']
}
```





## 路由的params参数

1）配置路由，声明接收params参数

```javascript
{
    path: '/home',
    component: Home,
    // 子路由
    children:[
        {
            path: 'news',
            component: News
        },
        {
            path: 'message',
            component: Message,
            children: [
                {
                    name: 'xiangqing',
                    //path使用占位符来接收params参数
                    path: 'detail/:id/:title',
                    component: Detail
                }
            ]
        },                
    ]
},
```

2）传递参数

**编程式路由导航，对象里内容写法一致**

```html
<!-- 跳转并携带params参数，to的字符串写法 -->
<router-link :to="/home/message/detail/666/你好">跳转</router-link>

<!-- 跳转并携带params参数，to的对象写法 -->
<router-link
:to="{
     name:'xiangqing',
     params:{
     	id:666,
     	title:'你好'
     }
     }"
>跳转</router-link>
```

3）接收参数

传递给那个页面了，就在那个页面里接收数据

```js
//参数都传递到了$route.params中
$route.params.id
$route.params.title
```



**特别注意**：**路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置**



**一、params的问题：**

如果已经配置了params参数

```js
//在路由中已经占位了
{
    name: 'xiangqing',
    //path使用占位符来接收params参数
    path: 'detail/:id/:title',
    component: Detail
}
```

但是并没有传递params参数，**会导致路由的路径发生问题**

**二、如何指定params参数可传可不传？**

在参数后面加上？

```js
//在路由中已经占位了
{
    name: 'xiangqing',
    //path使用占位符来接收params参数,并加上俩?标识可传可不传
    path: 'detail/:id?/:title?',
    component: Detail
}
```



**三、params参数可传可不传，但是传递的时空字符串怎么解决问题？**

当配置路由在后面加上?时，虽然解决了不传值的问题，但是如果传递了一个空字符串''，任然会有**路由路径问题**

解决方式，利用或的短路求值，？可以判断undefined

```js
this.$router.push({
    name: `search`,
    params: {
    // 如果时空串，则返回的时undefined，？可以判断出来
    keyword: '' || undefined//解决空串问题
    },
    query: {
    k: this.keyword
    }
})
```





## 路由的props配置

**props的配置是写在路由里的奥**

```javascript
{
    name: 'xiangqing',
    path: 'detail',
    component: Detail,
    // props的第一种写法 值为对象,该对象中的所有key-value都会以props的形式传给Detail组件
   	props:{a:1, b:'hello'}

    // props的第二种写法 值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件,注意传递的params参数都是string类型的
    props:true

    // props的第三种写法 值为函数,以props的形式传给Detail组件，
    props($route) {
        return {
            id: $route.query.id, 
            title: $route.query.title
        };
    }
}
```

第三种好用(●'◡'●)



**接收的时候和父子组件的props参数一样**

示例：

```vue
props:['id','title']

// vue3.2
defineProps(['id','title'])
```









## `<router-link>` 的replace属性

**replace和push的区别，编程式路由导航也是这样**

1）作用：控制路由由跳转时操作浏览器历史记录的模式。

2）浏览器的历史记录有两种写入方式：分别是`push`和`replace`.

`push`是追加历史记录，`replace`是替换当前记录。路由跳转时候默认为`push`

3）如何开启`replace模式`：`<router-link replace ....>News</router-link>`，**在router-link中加一个replace属性**

补充：其实历史记录的模式是一个堆栈，每次新加一条记录就是在栈顶加一条

## 编程式路由导航(非常重要，用的比较多)

声明式导航能做的，编程式导航都能做，并且编程式导航还能做一些其他操作

1）作用：不借助`<router-link>`实现路由的跳转，让路由跳转更加灵活。

2）具体代码：

```vue
<button @click="pushShow(m)">push查看</button>
<button @click="replaceShow(m)">replace查看</button>

<script>
// router的两个api
methods: {
    pushShow(m) {
        //如果单纯跳转.push('/xxx')即可
        this.$router.push({
            name: 'xiangqing',
            query: {
                id: m.id,
                title: m.title
            }
        })
    },
    replaceShow(m) {
        this.$router.replace({
            name: 'xiangqing',
            query: {
                id: m.id,
                title: m.title
            }
        })
    },
} 
</script>
```



3）$router上还有几个好用的API，前进，后退，指定跳转。

```javascript
<button @click="back">后退</button>
<button @click="forward">前进</button>
<button @click="go">测试以下go</button>


methods: {
    // 后退
    back() {
        this.$router.back();
    },
    //前进
    forward() {
        this.$router.forward();
    },
    //更具括号中的参数决定后退还是前进
    go() {
        this.$router.go(-1);
    }
}
```



### 编程式导航的小问题

问题一：编程式导航，进行多次路由跳转(参数不改变)，多次执行会抛出NavigationDuplicated异常

解决方式：在push传递参数时，后面跟两个处理函数(一个成功回调，一个失败的回调)

```js
      this.$router.push({
        name: `search`,
        params: {
          keyword: '' || undefined
        },
        query: {
          k: this.keyword
        }
      }, () => { }, () => { })
      // 就是在push末尾添加俩函数
```

还有个根本解决方式，重写以下VueRouter的push方法

在路由文件下写入以下代码：

```js
// 备份一份VueRouter的push方法
let originPush = VueRouter.prototype.push

// 重写以下push方法
VueRouter.prototype.push = function(location, resolve, reject) {
    // 如果传了resolve和rejecte就调用
	if (resolve && reject) {
		originPush.call(this, location, resolve, reject)
	} else {
        // 如果没传，我们自己传递俩函数
		originPush.call(
			this,
            location,
			() => {},
			() => {}
		)
	}
}

// replace方法重写和push一致，就是把push改为replace
```





## 缓存路由组件keep-alive组件

1）作用：让不展示的路由组件**保持挂载，不被销毁**，避免重新被渲染

2）具体编码：

```html
<keep-alive include="News">
    <router-view></router-view>
</keep-alive>

// include="组件名"是指定该组件不被销毁

如果要展示包含多个就写成数组
:include="['组件名','组件名'...]"
```



## 两个船新的生命周期钩子

1）左右：路由组件所**独有的两个钩子**，用于捕获路由组件激活的状态

2）具体名字

```javascript
// 激活
activated() {
    console.log('News组件被激活了');
    this.timer = setInterval(() => {
        console.log('定时器');
        this.opacity -= 0.01;
        if(this.opacity <= 0) this.opacity = 1;
    }, 16);
},
// 失活
deactivated() {
    console.log('New组件失活了');
    clearInterval(this.timer);
}
```

1. `activated`路由组件被激活时触发。
2. `deactivated`路由组件失活时被触发。



## 路由守卫

1）作用：对路由进行权限控制。

2）分类：全局守卫、独享守卫、组件内守卫

### 全局守卫：

写在路由文件中

```javascript

// 全局前置路由守卫---初始化时被调用，每次路由切换之前被调用
router.beforeEach((to, from, next) => {
    console.log('路由被切换了',to, from);
    // 判断是否需要鉴定权限
    if(to.meta.isAuth) {
        if(localStorage.getItem('school')  === 'rehaid'){
            next();
        } else {
            alert('学校名错误，无权限查看');
        }
    } else {
        next();
    }
})

// 导航守卫执行完毕、且组件加载完毕、组件中的导航守卫执行完毕之后、且导航实际跳转前执行。适合加载全局数据，或者在beforeEach做数据权限鉴定完毕后，还有一些特定权限的可以在beforeResolve中实现
router.beforeResolve((to) => {
})

// 全局后置路由守卫---初始化时被调用，每次路由切换切实际跳转之后被调用，这时页面dome已经渲染完毕，可以修改例如：document.title = to.path;
router.afterEach((to, from) => {
    console.log('后置路由守卫',to, from);
    document.title = to.meta.title || '练习';
})


// to:中有meta，path等信息，next()是执行跳转
```

### 独享守卫：

如果导航守卫返回false，则会阻止导航的跳转。

```javascript
//独享路由是写在路由内部的，里面内容和全局前置路由一致
{
    name: 'xinwen',
    path: 'news',
    component: News,
    meta: {isAuth: true, title: '新闻'},
    // 路由跳转时，组件创建前执行,不能访问组件的实例
    beforeEnter:(to, from, next) => {
        console.log('路由被切换了',to, from);
        // 判断是否需要鉴定权限
        if(to.meta.isAuth) {
            if(localStorage.getItem('school')  === 'rehaid'){
                next();
            } else {
                alert('学校名错误，无权限查看');
            }
        } else {
            next();
        }
    },
     // 也可以写成数组模式,数组模式就可以写多个验证函数了
    beforeEnter: [auth]
}

// 将验证逻辑抽离成一个函数
function auth = (to, from) => {
  xxxx
}
```

注意：**该守卫只有从不同的url跳转进来，才会进行触发。而向动态路由`/:postId`这种的，从`/1`跳转到`/2`就不会触发**



### 组件内路由守卫：

写在组件中，可以给组件添加一些独有的判断时写入

```javascript
//通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {
    console.log('通过路由规则进入',to,from);
    // 判断是否需要鉴定权限
    if(to.meta.isAuth) {
        if(localStorage.getItem('school')  === 'rehaid'){
            next();
        } else {
            alert('学校名错误，无权限查看');
        }
    } else {
        next();
    }
},

//通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {
    console.log('通过路由规则离开',to,from);
    next();
}
```



## 路由器的两种工作模式

1）对于一个url来说，什么是hash值？—#及其以后的内容就是hash值

2）hash值不会包含在HTTP请求中，即：hash值不会带给服务器。

3）hash模式：

1. 地址中永远带着#号，不美观。
2. 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
3. 兼容性较好

4）history模式：

1. 地址干净，美观。
2. 兼容性和hash模式相比略差。
3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。

history解决方法之一：npm搜connect-history-api-fallback这个中间件可以解决。

## 路由重定向redirect

redirect是重定位配置项，可以将你现在访问到的路径重新定位到redirect后的路径

```js
  {
    path: '/',
    redirect: '/login'
  }
//标识访问更目录就跳到/login

// 第二种写法：对象式
{
  path: '/user',
  // 重定向到home
  redirect: {
    name: 'home'
  }
}


// 第三种写法：函数式
{
  path:'/posts/:postId',
  redirect: (to) => {
    return {
      path: `/${to.params.postId}`
    }
  }
  // 如果直接写成：redirect: '/:postId'会出错，他会将/:postId直接当作URL进行跳转，并不会接收参数
}
```



## 路由元信息（meta）

`meta:{xxx}`为路由下的一个配置项，配置过后会添加到$route上，可以存放一些标识信息，用`$route.meta.xxx`来访问

有时候会结合路由守卫呀，v-show呀等等一起使用

```js
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // 只有经过身份验证的用户才能创建帖子
        meta: { requiresAuth: true }
      },
      {
        path: ':id',
        component: PostsDetail
        // 任何人都可以阅读文章
        meta: { requiresAuth: false }
      }
    ]
  }
]
```

配合全局守卫进行权限控制：

```js
router.beforeEach((to, from, next) => {
    // 通过to.meta来访问
})
```

注意：`to.meta`是`to.matched`上所有顶级路由`meta`的浅合并出来的值，如果有子路由，还是建议判断`to.matched`身上的所有`meta`

如果一个父路由及其全部子路由都需要登陆后访问，那么我们只给父路由设置meta属性即可

## 路由懒加载

使用路由懒加载原因：当打包构建应用时，JavaScript包会变得非大，影响页面加载，如果能把不同路由对应的组件分割成不同的代码块，然后路由被访问时才加载对应的组件，这样可以优化效率。

当打包构建应用时

方式：`Promise`配合webpack的动态引入`import`

使用方式：

```js
// 原来路由中引入组件方式
import Foo from './Foo.vue';

// 路由懒加载方式
const Foo = () => import('./Foo.vue')


// 使用时 一致

const routes = [
{
	path: '/foo',
	component: Foo
}
]
const router = new VueRouter({
  routes: 
})
    
export default router;
```

**注意（官网原话）**：**如果您使用的是 Babel，你将需要添加 [`syntax-dynamic-import`](https://babeljs.io/docs/plugins/syntax-dynamic-import/)插件，才能使 Babel 可以正确地解析语法。**



## router.currentRoute

这是router身上的一个方法，可以通过该方法获得当前页面的**路由规则**

```js
const router = new VueRouter({
  routes
});

router.currentRoute
```



## path: "*"

在路由中，这个路径`path:"*"`表示除了已有路径之外的其他所有路径，一般都是将这条路径用作错误处理页面上

```js

// 静态路由
const routes = [
  {
    path: "/home",
    name: "Home",
    component: Home,
    // redirect: "/login",
    children: [
      // {
      //   path: "/menu/one",
      //   component: () => import("@/views/Page1.vue")
      // },
    ]
  },
  {
    path: "/login",
    name: "Login",
    component: Login
  },
  {
    // 错误处理 跳转到NotFound界面
    path: "*",
    name: "NotFound",
    component: NotFound
  }
];
```



## 动态添加路由

动态添加路由的一个关键是`router`身上的一个方法，可以动态添加路由规则

`router.addRoutes(currentRoutes);`

参数：`currentRoutes`的格式需要和`routes`一致

```js
const routes = [
  {
    path: "/home",
    name: "Home",
    component: Home,
    children: []
  }
]
```

详细操作可以看一看权限控制哪里



## route.matched方法

叫做给定路由当前标准化路由数组，就是获取当前访问路由的所有路由规则，返回的是一个数组

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
     	component:Foo,
      children: [
        {
          path: 'bar',
          component: Bar
        }
      ]
    }
  ]
})
```

如果当前访问的路由为`/foo/bar`，在当前路由组件中调用`this.$route.matched`方法会直接获取一个完整的对象的`routes`的副本，一般可以丢在面包屑中使用
