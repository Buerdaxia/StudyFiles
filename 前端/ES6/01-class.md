# class

## class 类

ES6 提供了更接近传统语言的写法，引入了Class（类）这个概念。作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都能做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

1. class 声明类
2. constructor 定义构造函数初始化
3. extends 继承父类
4. super 调用父级构造方法
5. static 定义静态方法和属性(**静态方法属于类，而不属于类的实例对象**)用类名来调用
6. 父类方法可以重写(**子类和父类方法同名时，使用子类方法，构造方法也会是重写的哦~，所以才出现了super方法**)
7. **类中的方法会自动开启局部严格模式**（'use strict'）

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
              'use strict' // 默认开启
                console.log("我可以打电话");
            }
            
            static eat() {
            	console.log('吃饭了');
              console.log(this);// this指向整个class：this-> class Shouji{...}
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

**注意点：**

1. 实例属性和实例方法都是给实例对象取调用的
2. 每一个实例对象在内存中都是独立的，各自拥有自己的属性和方法。
3. 静态方法,静态属性都是通过类名来调用的
4. 一般会把没有`this`关键字的方法定义为`static`方法
5. static方法种的this指向一整个class







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
				// 通过get可以直接引用函数的返回值，不用调用
        console.log(s.price);//s.price的值就是 上面方法的返回值
    
        s.price = "123";//赋值就调用set price了
```





## 类的简写

1. 类中能直接写赋值语句：赋值语句的内容会当作实例对象的属性

```js
class Car {
  // 如下代码意思：给Car的实例对象添加一个属性，名儿为a，值为1
  a= 1;
}
```

2. 类中可以直接给`Car`添加属性，只需要在上一部基础上，再加上一个关键字`static`

```js
class Car {
  // 如下代码意思：给Car添加一个属性，属性名为a，值为1
  static a= 1;
}
```

