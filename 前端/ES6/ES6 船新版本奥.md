# ES6 船新版本奥

## let声明

特性：

1. let 不能重复声明变量
2. 支持块级作用域
3. 不存在变量提升
4. 不影响作用域链效果

### 块级作用域

指let声明的变量只能在{}包裹的块级作用域下使用，外面无法访问。

if，else，while，for中用let声明也是块级作用域

## const声明

特性：

1. 必须赋值
2. 一般常量用大写(潜规则)
3. 常量值不能修改
4. 支持块级作用域
5. 用const声明的数组和对象**内的值**可以修改，不算做对常量的修改，不会报错（应为 对象和数组是引用类型，修改值并没有改变引用）

## let和const的暂时性死区

其实：let和const同样有变量提升的作用，但是区别于var的是。

var 在创建时就被初始化，并且赋值为undefined

而let/const在进入块级作用域后，会因为变量提升的原因先创建，但是并不会直接初始化，而是当声明语句结束后`let name =xxx;`才进行赋值，切默认值为`undefined`，而const必须初始化。所以说从创建到初始化的这段代码片段，就称为‘’**暂时性死区**‘’

一篇博客对ES6标准翻译出的一句话

**由let/const声明的变量，当它们包含的词法环境(Lexical Environment)被实例化时会被创建，但只有在变量的词法绑定(LexicalBinding)已经被求值运算后，才能够被访问**



## 解构赋值

ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值。

这一操作被称为：解构赋值

可以用到函数形参中，如果需要传入一个对象，就可以用解构

1）数组的解构

```javascript
    const name = ['钱不二','爽爷','涛哥','曹老板'];
    let [qian, wu, zheng, cao] = name;
    console.log(qian);// 钱不二
    console.log(wu);//爽爷
	//相当于
	/*
	let qian = '钱不二';
	let wu = '爽爷';
	let zheng = '涛哥';
	let cao = '曹老板';
	*/
	//只获取钱不二和涛哥不要的用空格代替
	let [qian1,,zheng1] = name;
```

数组的解构套娃版

```js
let arr = [1, 2, 3, [4, 5]];

//可以拿到二维数组值
let [a,b,c,[d,e]] = arr;

console.log(a,b,c,d,e);//12345

```





2）对象的解构

**对象解构时：大括号里的变量要和对象的属性名一致**

```javascript
    const NAME = {
        name: '钱不二',
        age: '22',
        p: function() {
            console.log('我是钱不二');
        }
    }
    /*
    相当于 let p = Name.p;
    */
    let {p} = NAME;// let声明的名字必须和NAME中属性名相同(同名赋值)

    p();//window.p()

	let {name:MyName} = NAME;
	//：是用来起别名,相当于拿出NAME中的name 赋值给MyName
	console.log(MyName);//'钱不二'
```

对象解构套娃版

```js

let name = {
    sex: {
        age: {
            tell: 100
        }
    }
}
//方式1 拿到tell
let {sex} = name;
let {age} = sex;
let {tell} = age;

//方式2 拿到tell
let {sex:{age:{tell}}} = name;

console.log(tell);
```



## 模板字符串

ES6声明字符串的新方式``(反引号), 在键盘的波浪号上

特性：

1. 可以在内容中直接出现换行符(就是原来不能敲回车，现在可以用``直接敲)
2. 可以拼接变量（${变量}，放在``里）

## 简化对象写法

ES6 允许在大括号里，直接写入变量和函数，作为对象的属性和方法(更简洁)

```javascript
    let name = '钱不二';
    let change = function () {
        console.log('欸，怎么说');
    }
    //同名可以直接丢尽对象中
    const school = {
        name,
        change
    }

    // 等效于
    const school = {
        name: name,
        change: change
    }
    
```

对象方法简写：

```javascript
const name = {
    setName: function() {
        console.log('123');
    }
}
// 等效于
const name = {
    //: 和 function都能不写
    setName(){
        console.log('123');
    }
}
```

## 箭头函数(用的非常多)

ES6 允许使用箭头定义函数：=>

语法：

```javascript
let fn = (参数一,参数二...) => {
    函数体
};
// 调用方式不变
fn();
```

**特性**：

1. 箭头函数this是静态的！(**this始终指向函数声明时所在作用域下的this值**)很重要
2. 不能作为构造函数实例化对象
3. 不能使用arguments变量(**arguments保存传入的实参**)
4. 箭头函数还能简写(●'◡'●)

```javascript
//1.当箭头函数只有一个参数时可以不写小括号
// 正常版
let fn = (n) => {};

//省略小括号版
let fn = n => {};

//2. 当箭头只有一条语句时可以省略花括号,而且return必须省略
//正常版
let fn2 = (n) => {
    return n*n;
}

//省略版
let fn2 = n => n*n;//没有return，返回值就是语句执行的结果
```

```js
//箭头函数的4中形式
// 无参数 无返回值
let func = () => console.log('hello');

// 无参数 有返回值
let func = () => 9+1;

// 有参数 无返回值
let func = (x) => console.log(x);

// 有参数 有返回值
let func = (x, y) => x+y;
```



箭头函数好用的地方：适合与this无关的回调，定时器，数组的方法回调

箭头函数不合适的地方：不适合与this有关的回调、事件回调、对象的方法

## ES6允许给函数形参加初始值

```javascript
function add(a, b, c=10) {
    // 一般将有默认值的参数丢在最后(潜规则)
}
```

2）与解构赋值组合使用：

```js
function connect({host="127.0.0.1", username="root", password, port}) {
            console.log(host);
            console.log(username);
            console.log(password);
            console.log(port);
        }
let obj = {
        host: 'localhost',
        username: 'root',
        password: 'root', 
        port: 3306       
        }
//传入则为obj的值
connect(obj);
//没传入，用默认值
connect();
```

```js
//解决没传入对象报错问题
function func({name, age} = {}) {
    // 如果没有传入参数，就使用默认值name.age为undefined
    console.log(name, age);
}

func();
```





## rest参数

rest参数，用于获取参数的实参，用来代替arguments

```javascript
function data(...args){
    console.log(args);
}
data('钱不二', '不二大侠');
```

特性：

1. 必须要有...点表明他是一个rest参数，**并且是在函数形参位置声明**
2. 是用一个**真**数组保存实参(可以使用数组的方法)
3. 有多个形参时，rest参数必须放在最后

## 扩展运算符 ...

... 扩展运算符能将将数组转换为逗号分隔的参数序列。

注意：扩展运算符只能展开拥有迭代器`iterator`属性的数据结构，**不能展开对象**

```javascript
let name = ['钱不二', '倩儿', '不二大侠'];
function getName() {
            console.log(arguments);
        }
getName(...name);// 等同于getName('钱不二', '倩儿', '不二大侠');
```

特性：

1. 数组合并

```javascript
1.数组的合并
const kuaizi = ['王太利', '肖央'];

const fenghuang =['曾毅', '玲花'];
        // 利用concat合并
        const zuixuanxiaopingguo = kuaizi.concat(fenghuang);
        // 利用扩展运算符合并
        const zuixuanxiaopingguo = [...kuaizi, ...fenghuang];
        // 后面语句意思： '王太利', '肖央','曾毅', '玲花'
```

2. 数组克隆

```javascript
        // 2. 数组的克隆
        const sanzhihua = ['E', 'G', 'M'];
        const sanyecao = [...sanzhihua];
        console.log(sanyecao);
```

3. 将伪数组转换为真数组

```javascript
//这种方式有时候也叫剩余参数
function getName(...params) {
	console.log(params);//是一个数组['钱不二','不二大侠']
}
getName('钱不二','不二大侠')
```

4. 对象合并（使用的不是...展开运算符）

```javascript
let a = {name:'钱不二',age: 18, sex: '男'};
let b = {age: 22, sex: '女'};
// 注意这种写法是触发了构造字面量对象展开的特殊写法，并不是用到了展开运算符，而是一种固定写法
let info = {...a,...b};
console.log(info);//将a和b一起合并，一起有的值以后面的b为准
```

**如果有相同属性名，属性会被后面的覆盖**



## Symbol 原始类型

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript的第七种数据类型，是一种类似于字符串的数据类型。

symbol特性：

1. symbol 的值是唯一的，用来解决命名冲突的问题
2. symbol 值不能与其他数据进行运算
3. symbol 定义的对象属性不能使用for...in循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

symbol还有十一个内置属性，这些内置属性又是写到对象上的，用来扩展对象功能的。这11个属性查资料自己看

(￢︿̫̿￢☆)

## 迭代器(iterator) 

迭代器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署iterator迭代器接口，就可以完成遍历操作。

1. ES6 创造了一种新的1遍历命令 for...of循环，迭代器iterator接口主要提供for...of消费

2. 原生具备迭代器接口的有:

   **Array, Arguments, Set, Map, String, TypedArray, NodeList**

迭代器的工作原理：

1）创建一个指针对象，指向当前数据结构的其实位置

2）第一次调用对象的next方法，指针自动指向数据结构的第一个成员

3）接下来不断调用next方法，指针一直向后移动，直到指向最后一个成员。

4）每次调用next方法返回一个包含value和done属性的对象



注意：**想要自定义遍历数据的时候，要想到迭代器**





## for...of原理

如果将`for...of`循环拆解成最原始的for循环，内部实现机制可以这么理解

```js
let arr = [1, 2, 3, 4, 5];
let iterator = arr[Symbol.iterator]();
for(let res, value; (res = iterator.next()) && !res.done;) {
  value = res.value;
}
```

## 生成器

生成器是ES6提供的一种异步编程解决方案。与传统方法完全不同。

生成器就是一个特殊的函数

```javascript
function * gen() {
   console.log("我是生成器函数"); 
}
// 要在函数名前加上*

//调用
let iterator = gen();
iterator.next();
```



next()也能传递参数：

```javascript
        function * gen(arg) {
            console.log(arg);
            let one = yield 111;
            console.log(one);
            let two = yield 222;
            console.log(two);
            let three = yield 333;
            console.log(three);
        }
        let iterator = gen('aaa');
        console.log(iterator.next()); // 第一个调用next作用都是为了起步而已

        console.log(iterator.next('bbb'));//第二个next 传入的实参作为第一个yield的返回结果

        console.log(iterator.next('ccc'));//第三个next 传入的实参作为第二个yield的返回结果

        console.log(iterator.next('ddd'));//第四个next 传入的实参作为第三个yield的返回结果
    
```

## Set 集合

ES6提供了新的数据结构Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了iterator接口，所以可以使用扩展运算符和【for...of...】进行遍历。

集合的属性和方法：

1. size 返回集合元素个数
2. add(参数) 增加一个新元素，返回当前集合
3. delete(参数) 删除一个元素
4. has(参数) 检查有没有这个元素，有返回true否则返回false
5. clear() 清空集合 

参数：字符串

```javascript
        // 声明一个set
        let s = new Set();
        let s2 = new Set(['大事儿','小事儿','好事儿','小事儿']);
        // 可以自动去重
        console.log(s2);

        // 个数
        console.log(s2.size);
        // 添加新元素
        s2.add('喜事儿');
        console.log(s2);

        // 删除元素
        s2.delete('小事儿');
        console.log(s2);

        // 检查元素
        console.log(s2.has('大事儿'));

        // 清空元素
        // s2.clear();
        // console.log(s2);
        // 遍历
        for(let v of s2) {
            console.log(v);
        }
```



## Map

ES6 提供了Map数据结构，它类似于对象，也就是键值对的集合。但是 键的范围不在限制于字符串，各种类型的值（包括对象）都可以当作键。Map也实现了iterator接口，所以可以使用【扩展运算符】和【for...of...】进行遍历

Map的属性和方法：

1. size 返回Map的元素个数
2. set 增加一个新元素，返回当前Map
3. get 返回键名对象的键值
4. has 检查Map是否包含某个元素，返回Boolean值
5. clear 清空集合，返回undefined

```javascript
        // 声明
        let m = new Map();
        
        // 添加元素
        m.set('name', '钱不二');
        m.set('change', function(){
            console.log('芜湖');
        });
        let key = {
            school: '学校'
        }
        m.set(key, ['北京', '南京']);

        // 获取数量
        console.log(m.size);

        // 删除
        m.delete('name');

        // 获取
        console.log(m.get(key));

        // 清空
        m.clear();
        console.log(m);

        // 遍历
        for(let v of m) {
            console.log(v);
        }
```



## class 类

ES6 提供了更接近传统语言的写法，引入了Class（类）这个概念。作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都能做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

1. class 声明类
2. constructor 定义构造函数初始化
3. extends 继承父类
4. super 调用父级构造方法
5. static 定义静态方法和属性(**静态方法属于类，而不属于类的实例对象**)用类名来调用
6. 父类方法可以重写(**子类和父类方法同名时，使用子类方法，构造方法也会是重写的哦~，所以才出现了super方法**)

```javascript
//ES5       
function Phone(brand, price) {
            this.brand = brand;
            this.price = price;
        }
        // 添加方法
        Phone.prototype.call = function() {
            console.log('我可以打电话');
        }
        // 实例化对象
        let HuaWei = new Phone('华为', '5999');
        HuaWei.call();
        console.log(HuaWei);

        // class ES6
        class Shouji{
            // 构造方法
            constructor(brand, price) {
                this.brand = brand;
                this.price = price;
            }

            // 方法必须使用该语法
            call() {
                console.log("我可以打电话");
            }
            
            static eat() {
            	console.log('吃饭了');
       		}
        }
		// Shouji的静态属性 size
		Shouji.size = '200g';
        let onePlus = new Shouji('1+', "2999");
		onePlus.call();
		Shouji.eat();//调用静态方法，通过类名调用
		console.log(Shouji.size);//调用静态属性也需要通过类调用
        console.log(onePlus);
```

注意点：

1. 实例属性和实例方法都是给实例对象取调用的
2. 每一个实例对象在内存中都是独立的，各自拥有自己的属性和方法。
3. 静态方法,静态属性都是通过类名来调用的
4. 一般会把没有`this`关键字的方法定义为`static`方法



## 类进行继承操作

```javascript
        // 类的继承
		//	父类
        class Phone {
            // 构造方法
            constructor(brand, price) {
                this.brand = brand;
                this.price = price;
            }
          // 外面的方法和属性是公共方法，加在原型prototype上的
            call() {
                console.log("我可以打电话");
            }
        }

      //子类  
        class smartPhone extends Phone {
            // 构造方法
            // 由于构造方法名一致，所以重写了，所以内部要调用super来拿到父元素的属性
            constructor(brand, price, color, size) {
                // super就是父类的constructor 方法
                super(brand, price);
                // 自带的独有属性
                this.color = color;
                this.size = size;
            }

            // 自带方法
            photo() {
                console.log("我能拍相片");
            }
        }
		// 执行这条语句的步骤：
		/*1.创建一个空对象
		  2.执行constructor方法
		  3.让xiaomi指向该对象
		*/
        let xiaomi = new smartPhone("xiaomi", 1999, "black", "4.7inch");//实参和构造方法形参一一对应
        console.log(xiaomi);
```

### 继承中的注意点（写法要求）：

1. **在继承这个子类中，如果子类出现和父类方法名一样的方法，就是重写，重写以子类为准**
2. 因为子类中构造函数中`constructor`没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用`super()`方法，子类就得不到this对象，使用this就会报错
3. **子类构造方法中，this要在super()方法之后（否则会报错）**
4. 如果子类中也有`constructor`则会重写父类的`constructor`方法，则父类的`constructor`属性全部失效，如果想要得到就必须使用`super()`方法

所以：**如果子类中有constructor，则必须调用super方法（还要支持上面条件3），如果子类constructor需要传递参数，则super也要传**





## class中的get和set

这个`class`中的`get`和`set`和`Object.defineProperty()`方法很像，底层应该调用的就是这个`api`

```js
        // get和set
        class Phone {
            get price() {
                console.log("价格方法被调用了");
                return "价格";
            }
            
            set price(newValue) {
                console.log("价格被赋值了");
            }
        }
        
        
        let s = new Phone();
        console.log(s.price);//s.price的值就是 上面方法的返回值
    
        s.price = "123";//赋值就调用set price了
```

## 数值扩展

Number.EPSILON: 用于判断浮点数计算的。

```javascript
        // Number.EPSILON
        function euqal(a, b) {
            if(Math.abs(a - b) < Number.EPSILON){
                return true;
            } else {
                return false;
            }
        }
        console.log(0.1 + 0.2 === 0.3);
        console.log(euqal(0.1 + 0.2, 0.3));
```

Number.isFinite: 检测一个数是否为有限数



Number.isNaN 检测一个数值是否为NaN

```javascript
console.log(Number.isNaN(123));
```



Number.parseInt 和Number.float字符串转换

```javascript
        console.log(Number.parseInt("123aaa"));
        console.log(Number.parseFloat("123.23bbb"));
```



Number.isInterger 判断一个数是否为整数

```javascript
console.log(Number.isInteger(2));//true
console.log(Number.isInteger(2.5));//false
```



Math.trunc 将数字小数部分抹掉

```javascript
console.log(Math.trunc(2.4));// 2
```



Math.sign 判断一个数是正数 0 还是负数

```javascript
        console.log(Math.sign(10));// 返回1
        console.log(Math.sign(0));// 返回0
        console.log(Math.sign(-10));// 返回-1
```



## 对象方法扩展

Object.is 判断两个值是否完全相等

```javascript
// 1.Object.is 判断两个数值似乎否完全相等
console.log(Object.is(123, 123));//true
console.log(Object.is(NaN, NaN));// true
console.log(NaN === NaN); // false

// 可以用来判断 NaN
```

 Object.assign 对象的合并

```javascript
        const config1 = {
            host: 'localhost',
            port: 3306,
            name: 'root',
            pass: 'root'
        };
        const config2 = {
            host: 'http://',
            port: 33060,
            name: 'qianbuer',
            pass: "qianbuer"
        }
        // 后面的把前面的覆盖
        Object.assign(config1, config2);
        console.log(config1);
```

Object.setPrototypeOf 设置原型对象 Object.getPrototypeOf获取原型对象

注意：不建议这么操作

**Object.defineProperty 方法：**(重点)

特性：

1）**改方法添加的属性默认下不能被遍历到**

**要添加一个enumerable: true才能被遍历**



2）默认该方法添加的属性值不能被修改，

需要添加一个writable: true才能修改



3）默认该方法添加的属性不能被删除

需要添加一个configurable: true才能删除

```javascript
Object.defineProperty(参数一, 参数二, {

​            value:参数三

​        })
参数一：要添加属性的对象
参数二：要添加的属性名
参数三：要添加的属性值
```

案例：

```javascript
Object.defineProperty(person, 'age', {
	value:18,
    enumerable: true,//允许被遍历
    writable: true,//允许属性可以被修改
    configurable: true//允许属性被删除 
})
```

Object.keys(对象名)：

返回一个由对象属性名组成的数组

```javascript
        let person = {
            name: '张三',
            sex: '男'
        }
        console.log(Object.keys(person));
//['name', 'sex']
```

## 对象方法中的get和set

```javascript
Object.defineProperty(person, 'age', {
    // value:18,
    // enumerable: true,//允许被遍历,默认为false
    // writable: true,//允许属性可以被修改，默认为false
    // configurable: true,//允许属性被删除，默认为false

    // 当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
    get: function() {
        console.log('有人读取了age属性');
        return number;
    },
    // 当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
    set:function(value) {
        console.log('有人修改了age的属性,且值是',value);
        number = value;
    }
})
```



## 模块化（重点）

模块化指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来。

模块化优势：

1. 防止命名冲突
2. 代码复用
3. 高维护性

## async

1. async函数的返回值是一个promise对象
2. promise对象的结果由async函数执行的返回值决定（就是由return语句决定）
3. 如果async函数状态没有改变，则返回一个`pending`状态`Promise`对象为（例如函数中有`await`，因为`await`会阻塞代码哟）

```
async function fn() {
      // 
      return '嘻嘻嘻'
}

```

三种情况：
1：如果返回的不是一个promise对象

则async函数返回的是一个成功的`promise`对象

2: 如果抛出错误`throw new Error('出错了')`

```js
async function fn() {
      throw new Error('出错了');
}
const result = fn();
console.log(result);//返回失败的promise对象 rejected
```



则async函数返回一个失败的promise对象

3：如果返回了一个promise对象

则asyce函数返回的promise状态和`return new Promise(...)`的这个对象状态一致

```js
//成功的promise
async function fn() {
      return new Promise((resolve, reject) => {
        resolve('成功了');
      })
}
const result = fn();
console.log(result);//返回成功的promise对象值为成功了
```

```js
//失败的promise
async function fn() {
      return new Promise((resolve, reject) => {
        reject('失败了');
      })
}
const result = fn();
console.log(result);//返回失败的promise对象值为失败了
```



## await

1. await必须写在async函数中（“单相思”,async不是必须要await）
2. **await右边的表达式一般为一个promise对象**
3. await返回的是promise成功的**值**（注意！是值）
4. await的promise失败了，就会抛出异常用`try...catch`捕获处理

```js
// 成功的promise
const p = new Promise((resolve, reject) => {
      resolve('成功了');
    })

    async function fn() {
      let result = await p;
      console.log(result); //返回的是值：成功了
    }
	//调用fn
    fn();
```

**异常的捕获**

```js
// 失败的promise   
   const p = new Promise((resolve, reject) => {
      // resolve('成功了');
      reject('失败了');
    })

    async function fn() {
      // 失败的用try...catch
      // 内部捕获
      try {
        let result = await p;
        console.log(result);
      } catch (e) {
        // e是await抛出的失败的值
        console.log(e);// 返回失败了
      }
    }

    fn();

// 外部捕获
fn().catch(err => {
  console.log(err);
})
```



## async与await的注意事项

一：如果await后方跟着一个基本数据类型，则会将这个基本类型包装成一个Promise

```js
async function func() {
	let data1 = await 123;
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('data1: ', data1);
	return data1;
}

let ret = func();
console.log('ret:', ret);
```

二：await会阻塞后续的代码

```js
async function func() {
	let data1 = await 123; // 之后的代码会被阻塞
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('data1: ', data1);
	return data1;
}

let ret = func();
// 这行console.log会优先执行
console.log('ret:', ret);
```



例如：

```js
// 求下列console.log()的执行顺序
async function func() {
	console.log('3')
	let data1 = await 123; // 之后的代码会被阻塞
	/*data1相当于，new Promise((resolve,reject) => {
    resolve(123);
  })
  */
	console.log('4');// 4这里就被await阻塞了，因为await是异步的
	return data1;
}
console.log('1')
func();
console.log('2');

// 执行顺序1，3，，2，4
```

