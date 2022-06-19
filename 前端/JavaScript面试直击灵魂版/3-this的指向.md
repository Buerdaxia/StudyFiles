# this

重点：**`this`是执行主体，关键在于谁把它执行了**。与`this`在哪里创建&在哪执行都没有必然的关系



## this的情况一，函数执行

函数执行：

看方法前面是否有`.`如果有就是谁调用的就是谁，如果没有点`.`则`this`非严格模式下是`window`严格模式下为`undefined`

注意：`a.b.fn()` 这种情况下fn的this是指向b的

代码：

```js
const fn = function () {
  console.log(this);
}
let obj = {
  name: 'obj',
  fn: fn
}
fn(); // this为window
obj.fn(); // this为obj
```



## this的情况二，事件绑定

给当前元素绑定事件方法时，当事件行为触发，方法中的`this`就是当前元素本身（就是当前的dom元素）（排除`attachEvent`）

代码：

```js
document.body.addEventListener('click', handler)
    function handler() {
      console.log(this); // this为body dom元素
    }
```



## this的情况三，构造方法

构造方法中的`this`，是指向调用构造方法创建出的实例

代码：

```js
function Factory() {
  this.name = '我';
  this.age = 23;
  console.log(this);// this指向f这个实例对象
}
let f = new Factory();
```



## this的情况四，箭头函数

箭头函数中没有执行主体，所用到的this都是其所处上下文中的`this`

代码：

```js
let demo = {
  fn() {
    console.log(this);

    setTimeout(() => {
      console.log('箭头函数:', this); //this为window
    }, 1000);

    setTimeout(function () {
      console.log('普通函数', this); //this 为demo
    }, 1000);
  }
}
demo.fn();
```



## call(),apply(),bind()方法

这三种方法的本质区别：call和apply除了参数不同其余一致，bind方法比较特殊

直接上代码：

```js
    function func(x, y) {
      console.log(this, x, y);
    }
    let obj = {
      name: 'OBJ'
    }
    /*
      call方法执行过程：
      1.  首先func函数基于_proto_找到Function.prototype.call方法，并执行call
      在call方法内部执行时 call(context->obj, ...params){}
      2.  将func方法中的this改为obj
      3.  并将params接受的参数传递给func
      4.  让func函数立即执行
    */
    func.call(obj, 10, 20);

    /*
      apply方法执行过程：
      1.  首先func函数基于_proto_找到Function.prototype.apply方法，并执行apply
      在apply方法内部执行时 apply(context->obj, params){}
      2.  将func方法中的this改为obj
      3.  并将params接受的参数(是数组)内容传递给func
      4.  让func函数立即执行
    */
    func.apply(obj, [10, 20]);

    /*
      bind方法执行过程：（不会立即执行func）
      1.  首先func函数基于_proto_找到Function.prototype.bind方法，并执行bind方法
      在apply方法内部执行时 
      2.  将传递进来的obj,10，20存储起来（形成闭包）
      3.  执行bind返回一个新的函数 例如：proxy，将proxy绑定给元素事件，当事件触发时
      执行的是proxy，在proxy的内部再去将func执行，把this和值都改为第二步保存的值
    */
    document.body.addEventListener('click', func.bind(obj, 10, 20));
```

### call()方法的手撕

apply方法和call方法原理基本一致。

直接上代码：

```js
Function.prototype.call = function (context, ...params) {
  /*
    this/self->func, context->obj ,...params->[10, 20]
  */
  // 核心： 需要将func的this改为obj，让obj去调用func不就行了？
  let self = this,
    key = Symbol('KEY');

  // 判断传入context类型问题
  context == null ? context = window : null;
  // 如果传入的是基本类型例如10，'1'等,利用Object()包装对象
  !/^(object|function)&/i.test(typeof context) ? context = Object(context) : null;
	
  // 给obj身上添加一个symbol属性并且是func
  context[key] = self;
  // 调用func并传递参数
  let result = context[key](...params);
  // 删除添加的属性
  delete context[key];
  // 最后一步，将函数结果返回
  return result;
}
/*
  call方法执行过程：
  1.  首先func函数基于_proto_找到Function.prototype.call方法，并执行call
  在call方法内部执行时 call(context->obj, ...params){}
  2.  将func方法中的this改为obj
  3.  并将params接受的参数传递给func
  4.  让func函数立即执行
*/
func.call(obj, 10, 20);
```

需要包装对象的原因：（解释上面12行的代码）

应为如果给一个基本数据类型添加属性，是运行添加属性的，但是访问不到

例如：

```js
let a = 10;
a.name = '111';
console.log(a.name); // undefined

// 使用Object(a)之后Object 构造函数将给定的值包装为一个新对象。然后就能添加属性了
Object(a);
a.name = 100;
console.log(a.name); //  100
```



### bind()方法手撕

直接上代码：

```js
Function.prototype.bind = function (context, ...params) {
  // this/self -> func, context -> obj, params-> [10, 20]
  let self = this;
  
  // 如果绑定的函数还有值传递，我们也要给他传递过去
  return function proxy(...args) {
    // 返回的函数要调用func切修改func的this
    self.apply(context, params.concat(args));
  }
}
/*
  bind方法执行过程：（不会立即执行func）
  1.  首先func函数基于_proto_找到Function.prototype.bind方法，并执行bind方法
  在apply方法内部执行时 
  2.  将传递进来的obj,10，20存储起来（形成闭包）
  3.  执行bind返回一个新的函数 例如：proxy，将proxy绑定给元素事件，当事件触发时
  执行的是proxy，在proxy的内部再去将func执行，把this和值都改为第二步保存的值
*/
document.body.addEventListener('click', func.bind(obj, 10, 20));
```



## 借用方法，this修改的小技巧

众所周知维数组是类似数组结构，但是无法使用数组的所有方法，所以如果一个伪数组想用一些数组的方法，可以通过修改数组`this`指向来借用方法。

代码：

```js
function func2() {
  // arguments就是一个伪数组
  console.log(arguments);
  // arguments.slice() ->直接使用方法会报错

  // 通过修改this指向借用方法，大部分都可以
  let result = Array.prototype.slice.call(arguments);
  return result;
}
func2(10, 20, 30); // 返回结果[10, 20, 30]
```

