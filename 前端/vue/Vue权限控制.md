# Vue权限控制



## 1、权限相关概念

### 1.1 权限分类

* 后端权限

从根本上来说，前端的权限控制的仅仅是视图的展示，权限的核心是在服务器中的数据变化，所以后端往往才是权限的关键。后端权限可以控制某个用户能否查询数据能否进行数据的修改操作。

后端如何知道该请求是那个用户发过来的？

```
cookie
session
token
```

后端的权限设计RBAC

```
用户
角色
权限
```







* 前端权限

前端权限的控制本质上来说，仅仅控制**视图层的展示**和**前端所发送的请求**，但是权限控制的核心还是后端，前端仅仅起到**锦上添花**的作用。



### 1.2 前端权限的意义

如果仅仅从数据层面考虑，确实只需要后端控制权限即可，但是前端进行权限控制还是有以下几点好处！

1. 降低非法操作的可能性

例如，在页面中展示一个<span style="color: red">就算点击也会失败的按钮</span>，将会增加非法操作的可能性。

2. 尽可能排除不必要请求，减轻服务器的压力

没有必要的请求，操作失败的请求，不具备权限的请求，就不应该去发送，请求少了，服务器压力自然会下降

3. 提高用户体验

更具用户具备的权限，展示自己权限范围内的功能，避免在界面上展示没有必要的功能，让每个用户做好自己**分内之事**



## 2、前端权限控制思路

### 2.1 菜单(路由)的控制

在登录请求中，会得到权限数据，（需要后端返回数据支持），前端根据权限数据，展示对应的菜单，才能查看相关的页面

### 2.2 界面的控制

如果用户没有登录，手动在地址栏敲入管理界面的地址，则需要跳转到登录页面。

如果用户已经登录，可是手动敲入非权限内的地址，则需要跳转到404界面



### 2.3 按钮的控制

在某个菜单的界面中，还需要更具权限数据，展示出可以进行操作的按钮，例如删除、修改、增加



### 2.4 请求和响应的控制

如果用户通过非常规操作，比如通过浏览器调试工具，将某些禁用按钮变成启用状态，此时发送请求应该要被拦截



## 3、vue的权限控制

### 3.1菜单控制

这里一般会修改一下后端数据格式

```js
{
  id: 1,
  username: "普通用户",
  password: "normal",
  token: "abcdefghijklmnopqrstuvwxyz",
  rights: [
    {
      id: 1,
      authName: "一级菜单",
      icon: "icon-menu",
      children: [
        {
          id: 11,
          authName: "一级项目1",
          path: "/menu/one",
          rights: ["view", "edit", "add", "delete"]
        },
        {
          id: 11,
          authName: "一级项目2",
          path: "/menu/two",
          rights: ["view"]
        }
      ]
    }
  ]
}
```

这一部分的数据中，除了该用户的基本信息之外，还有两个字段很关键

* token：用于前端用户的状态保持
* rights：该用户具备的权限数据，一级权限对应一级菜单，二级权限对应二级菜单



由于需要更具返回的数据，动态渲染左侧菜单栏，数据再`Login.vue`中得到，但是再`Home.vue`中使用，所以可以采用`vuex`来进行维护

1. vuex中代码：

```js
export default new Vuex.Store({
  state: {
    breads: [],
    username: sessionStorage.getItem("username"),
    /*
      解决刷新菜单栏消失问题，
      由于JSON.parse不能处理undefined所以用短值语法写一个字符数组
    */
    rightList: JSON.parse(sessionStorage.getItem("rightList") || "[]")
  },
  mutations: {
    // 修改state中rightList
    setRightList(state, data) {
      state.rightList = data;
      // 放到sessionStorage
      sessionStorage.setItem("rightList", JSON.stringify(data));
    },
    setUsername(state, data) {
      state.username = data;
      sessionStorage.setItem("username", data);
    }
  },
  actions: {},
  modules: {}
});
```

2. Login.vue的代码

```js
methods: {
    // 导入vuex中的setUsername方法
    ...mapMutations(['setUsername']),
    submitForm() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.isLogin = true
          this.$api.login({
            username: this.userData.username,
            password: this.userData.password
          }).then(res => {
            this.isLogin = false
            if (!res.data) {
              this.$message({
                message: '用户名或密码错误！',
                type: 'error'
              })
              return false
            }
            console.log(res)
            // 调用vuex中的setRightList方法
            this.$store.commit('setRightList', res.data.rights);
            this.setUsername(res.data.username);
            this.$router.push('/')
          })
        }
        return false
      })
    }
  }
```

3. Home.vue中的方法

```js
import { mapState } from 'vuex';
export default {
  data() {
    return {
      // 左侧菜单数据
      menulist: [],
    }
  },
  created() {
    // 初始化menulist菜单栏数据
    this.menulist = this.rightList;
  },
  computed: {
    // 将vuex中的rightList映射成计算属性
    ...mapState(['rightList'])
  }
```



### 菜单控制的小bug

由于菜单的数据是由登陆后获得数据返回，然后丢进vuex中，当`Home.vue`页面刷新后，`vuex`中的数据也会进行初始化，所以会导致，`Home`页面菜单丢失问题。

解决方式：使用`sessionStorage`存储数据，之后再将`sessionStorage`初始化`vuex`



### 3.2 界面的控制

#### 问题一

正常时刻，一般是等待用户点击登录操作后，登录成功后跳转到管理页面平台。但是如果用户直接再网址中输入管理平台界面地址，应当判断用户是否登录，如果已经登录则可以直接跳转，否则应当跳转到登录界面。



* 如何判断是否登录

```js
sessionStorage.setItem('token', res.data.token);
```

* 什么时机判断？

当路由进行跳转之前，我们就可以判断该用户是否携带了`token`然后进行验证

路由前置守卫：

```js
const router = new VueRouter({
  routes
});
router.beforeEach((to, from, next) => {
  // console.log("to", to);
  // console.log("from", from);
  if (to.path === "/login") {
    next();
  } else {
    const token = sessionStorage.getItem("token");
    // console.log("我是token", token);
    if (!token) {
      next("/login");
    } else {
      next();
    }
  }
});
```



#### 问题二

解决完毕问题一后，虽然在登录之前的问题解决了，但是，登录之后的问题并未解决。

例如：一个普通用户登录成功后，在地址栏敲入`/roles`（前提：`roles`是管理员才有的模块），任然可以进行跳转。我们要做的就是，分工明确。普通用户所不具有的功能，在地址栏输入后无法访问（没有这个路由）

解决方式一：动态路由+后端返回数据支持(数据支持也可以前端做)，（前提1：首先已经解决菜单路由控制问题，将信息已经存储到`sessionStorage`中）

第一步：先将`/home`下所有子路由抽离出来

```js
// 抽离出单个路由 动态路由
const menuOne = {
  path: "/menu/one",
  component: () => import("@/views/Page1.vue")
};
const menuTwe = {
  path: "/menu/two",
  component: () => import("@/views/Page1.vue")
};

const menuThree = {
  path: "/menu/three",
  component: () => import("@/views/Page1.vue")
};
const menuFour = {
  path: "/menu/four",
  component: () => import("@/views/Page1.vue")
};

const menuFive = {
  path: "/menu/five",
  component: () => import("@/views/Page1.vue")
};

// 动态路由与数据字段映射
const ruleMapping = {
  "/menu/one": menuOne,
  "/menu/two": menuTwe,
  "/menu/three": menuThree,
  "/menu/four": menuFour,
  "/menu/five": menuFive
};

// 静态路由
const routes = [
  {
    path: "/home",
    name: "Home",
    component: Home,
    // redirect: "/login",
    children: [
      // 将所有子路由抽离出来
      // {
      //   path: "/menu/one",
      //   component: () => import("@/views/Page1.vue")
      // },
      // {
      //   path: "/menu/two",
      //   component: () => import("@/views/Page1.vue")
      // },
      // {
      //   path: "/menu/three",
      //   component: () => import("@/views/Page1.vue")
      // },
      // {
      //   path: "/menu/four",
      //   component: () => import("@/views/Page1.vue")
      // },
      // {
      //   path: "/menu/five",
      //   component: () => import("@/views/Page1.vue")
      // }
    ]
  },
```

第二步：书写一个动态添加路由的方法`initDynamicRoute`在路由模块中

```js
// 引入vuex
import store from "../store";
export function initDynamicRoute() {
  // 根据二级权限，对router进行控制
  console.log(router);
  // 获取当前 router管理的所有路由
  const currentRoutes = router.options.routes;
  // 根据登录返回的数据中的path属性，来动态添加路由
  store.state.rightList.forEach(item => {
    item.children.forEach(item => {
      // 对应的动态路由
      const temp = ruleMapping[item.path];
      // 然后将对应的路由推到 home页面子路由中
      currentRoutes[0].children.push(temp);
    });
  }); 
  // 重新设置路由规则(将动态路由添加)
  router.addRoutes(currentRoutes);
}
```

这里的`ruleMapping`是我们做的一个映射，用`rightList`下的`children`中的`path`属性值和动态路由一一对应

```js
// 动态路由
const menuOne = {
  path: "/menu/one",
  component: () => import("@/views/Page1.vue")
};
const menuTwe = {
  path: "/menu/two",
  component: () => import("@/views/Page1.vue")
};

const menuThree = {
  path: "/menu/three",
  component: () => import("@/views/Page1.vue")
};
const menuFour = {
  path: "/menu/four",
  component: () => import("@/views/Page1.vue")
};

const menuFive = {
  path: "/menu/five",
  component: () => import("@/views/Page1.vue")
};
// 动态路由与数据字段映射
const ruleMapping = {
  "/menu/one": menuOne,
  "/menu/two": menuTwe,
  "/menu/three": menuThree,
  "/menu/four": menuFour,
  "/menu/five": menuFive
};
```



第三步：考虑`initDynamicRoute()`方法的调用时机！，该方法是给指定用户添加指定的路由，所以肯定实在登录过后才能调用，但是这样又有一个小bug，当登录到平台后，刷新一下界面，`router`文件会自动加载一下，然后又回到初始化的时候了（**动态添加路由之前**）,这样就导致所有添加的路由都无效了。

**所以最佳调用时机**：肯定是每个组件创建后都调用一下进行调用，所以我们应该放到组件的头子(`App.vue`)身上

`App.vue`代码：

```js
// 引入方法， 注意引入方式
import { initDynamicRoute } from './router'
export default {
  name: 'app',
  components: {
  },
  created() {
    // 根据用户所具备的条件动态添加路由
    initDynamicRoute();
  }
}
```



### 3.3 按钮的控制

不同用户，在相同界面上的权限是不同的，因此我们需要根据用户的权限来对按钮进行控制，用户不具备权限时，我们就需要将按钮隐藏或者禁用。

解决建议：**将逻辑放到自定义指令之中**,然后给需要控制的按钮，添加自定义指令即可

解决前提：任然需要后端人员进行接口配合

首先后端数据格式：(登录接口返回的数据)

```js
{
  id: 1,
  username: "普通用户",
  password: "normal",
  token: "abcdefghijklmnopqrstuvwxyz",
  rights: [
    {
      id: 1,
      authName: "一级菜单",
      icon: "icon-menu",
      children: [
        {
          id: 11,
          authName: "一级项目1",
          path: "/menu/one",
          rights: ["view", "edit", "add", "delete"]// 关键在这里 
        },
        {
          id: 11,
          authName: "一级项目2",
          path: "/menu/two",
          rights: ["view"]
        }
      ]
    }
  ]
}
```



第一步：首先先在需要控制的按钮上添加自定义指令

```vue
<!-- action: 'edit'是标识这个是修改按钮，只有用户权限rights中有edit的才会考虑是否显示 -->
<!-- effect: 'disabled'是标识这个按钮是否会被禁用，如果权限中没有edit，则会被经用 -->
<!-- 主要逻辑在下面第三步中，自定义指令哪里-->
<el-button v-permission="{action:'edit', effect: 'disabled'}"
  type="primary"
  @click="handleUpdate(scope)">
  <i class="el-icon-edit"></i>
</el-button>
```

第二步：控制路由身上的元信息`meta`

首先要思考一下，我们是根据后端返回的数据中的`rights: ["view", "edit", "add", "delete"]`这条键值对来配合，而每个页面上都是需要独立控制按钮，所以比较方便的一种绑定方式，就是让它和路由的元信息进行捆绑

router文件

```js
import Vue from "vue";
import VueRouter from "vue-router";
import Home from "../views/Home.vue";
import Login from "../views/Login.vue";
import NotFound from "../views/NotFound.vue";

import store from "../store";

Vue.use(VueRouter);

// 动态路由
const menuOne = {
  path: "/menu/one",
  component: () => import("@/views/Page1.vue")
};
const menuTwe = {
  path: "/menu/two",
  component: () => import("@/views/Page1.vue")
};

const menuThree = {
  path: "/menu/three",
  component: () => import("@/views/Page1.vue")
};
const menuFour = {
  path: "/menu/four",
  component: () => import("@/views/Page1.vue")
};

const menuFive = {
  path: "/menu/five",
  component: () => import("@/views/Page1.vue")
};

// 动态路由与数据字段映射
const ruleMapping = {
  "/menu/one": menuOne,
  "/menu/two": menuTwe,
  "/menu/three": menuThree,
  "/menu/four": menuFour,
  "/menu/five": menuFive
};

// 静态路由
const routes = [
  {
    path: "/home",
    name: "Home",
    component: Home,
    children: []
  },
  {
    path: "/login",
    name: "Login",
    component: Login
  },
  {
    path: "*",
    name: "NotFound",
    component: NotFound
  }
];

const router = new VueRouter({
  routes
});

export function initDynamicRoute() {
  // 根据二级权限，对router进行控制
  console.log(router);
  const currentRoutes = router.options.routes;
  store.state.rightList.forEach(item => {
    item.children.forEach(item => {
      // 映射动态路由
      const temp = ruleMapping[item.path];
			// 绑定元信息（给每个页面绑定不同的元信息）
      temp.meta = item.rights;
      // 然后将对应的路由推到 home页面子路由中
      currentRoutes[0].children.push(temp);
    });
  }); // 设置路由规则
  router.addRoutes(currentRoutes);
}

export default router;

```





第三步：编写自定义指令

之后，我们可以通过判断自定义指令存储的数据，和该页面路由身上的元信息(`meta`属性)进行比对，如果有那就展示，**否则进行隐藏或禁用**

注意：**这是我们分装好的自定义指令，还是需要到main.js中引入一下否则不会加载**

创建`src/utils/permission.js`

```js
import Vue from "vue";
import router from "../router";
Vue.directive("permission", {
  // inserted当元素被插入到dom节点时调用
  /*  
    el:当前指令绑定元素
    binding：当前指令绑定的值
  */
  inserted(el, binding) {
    // console.log(el);
    // console.log(binding);
    // console.log(router.currentRoute);
    // 得到自定义指定绑定的值 （add）
    const action = binding.value.action;
    // 得到自定义指令绑定的值(disabled)
    const effect = binding.value.effect;
    // 判断，当前的路由所对应的组件中，判断用户是否具备action的权限
    if (router.currentRoute.meta.indexOf(action) == -1) {
      // 当在meta元数据中找不到这个参数时
      if (effect === "disabled") {
        // 将按钮进行禁用操作
        el.disabled = true;
        // 这个是element-ui禁用时特定的类，需要加上
        el.classList.add("is-disabled");
      } else {
        // 如果不禁用，那就直接将这个按钮删除
        // 找到当前节点的父节点，然后删除当前元素（我杀我自己）
        el.parentNode.removeChild(el);
      }
    }
  }
});

```

### 3.4 请求和响应控制

这部分的控制，我们需要用到`axios`自带的请求拦截器

**请求控制**

1. 除了登录请求之外，所有的请求都必须携带者`token`，这样服务器才能验证你的身份

```js
axios.interceptors.request.use(
  config => {
    // Do something before request is sent
    console.log(config.url);
    console.log(config.method);
    if (config.url !== "login") {
      // 如果不是登录请求，我们就应该在请求头中，加入token数据
      config.headers.Authorization = sessionStorage.getItem("token");
    }
  return config;
)}
```



2. 如果发出了非权限内的请求，应该直接在前端访问时就阻止（减轻后端服务器压力，没有必要的请求就别发送了，发送返回的失败），

这一部分操作:

* 必须要让后端配合，必须要采用restful风格参数
* 还要结合之前使用的路由元信息`meta`才能实现，否则实现不了（寄）

```js
import Axios from "axios";
import router from "../router";

const axios = Axios.create({
  // baseURL: process.env.NODE_ENV === 'development' ? '' : '',
});

const actionMapping = {
  get: "view",
  post: "add",
  put: "edit",
  delete: "delete"
};
axios.interceptors.request.use(
  config => {
    // Do something before request is sent
    console.log(config.url);
    console.log(config.method);
    if (config.url !== "login") {
      // 如果不是登录请求，我们就应该在请求头中，加入token数据
      config.headers.Authorization = sessionStorage.getItem("token");

      // 根据请求方式映射 行为方式
      const action = actionMapping[config.method];
      // 判断非权限内容的请求 应该直接阻止
      const currentRight = router.currentRoute.meta;
      // 判断当前请求行为
      if (currentRight && !currentRight.includes(action)) {
        alert("没有权限");
        // 直接返回失败的promise 阻止请求发起
        return Promise.reject(new Error("没有权限"));
      }
      // 后端数据有要求，必须是restful风格
      /*
        get请求     view
        post请求    add
        put请求     edit
        delete请求  delete
      */
      // add view edit delete
    }
    return config;
  },
  error => {
    // Do something with request error
    return Promise.reject(error);
  }
);
export default (url, method = "get", data = {}) => {
  return axios({
    url,
    method,
    data
  });
};

```



**响应控制**

**状态码需要和后端规定一个**

* 得到了服务器返回状态码401，代表token超时，或者被篡改了，此时应该强制跳转到登录界面

```js
axios.interceptors.response.use(
  response => {
    if (response.status === 401) {
      router.push("/login");
      // 把session清空
      sessionStorage.clear();
      // 把vuex清空
      window.location.reload();
    }
    // Do something before response is sent
    return response;
  },
  error => {
    // Do something with response error
    return Promise.reject(error);
  }
);
```



## 总结

前端权限的实现，必须要后端提供数据支持，否则无法实现，寄！

返回的权限数据的结构，需要前后端沟通协商，怎样的数据使用起来最方便



### 菜单控制

* 权限的数据需要在多组件之间共享，因此采用vuex
* 放置界面刷新，权限数据丢失，所以需要纯在sessionStorage中，并且需要两者同步



### 界面控制

* 路由的导航守卫可以防止跳过登录界面
* 动态路由可以让不具备权限的界面的路由规则直接存在(display: none)



### 按钮控制

* 路由规则中可以增加路由元数据meta
* 通过路由对象可以得到当前的路由规则，以及存储在此规则中的meta数据
* 自定义指令可以很方便的实现按钮控制



### 请求和响应控制

* 请求拦截器和响应拦截器的使用
* 请求数据返回格式，最好约定好，例如restful风格

