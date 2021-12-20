# webStorage

1. 存储内容大小一般支持5mb左右（不同浏览器可能大小不一）

2. 浏览器端通过Window.sessionStorage和Window.localStorage属性来实现本地存储机制

3. 相关API：1)`xxxStorage.setItem('key', value)`;

   该方法接收一个键值作为参数，会把键值对添加到储存当中，如果键名存在，则更新其对应的值。

   

   2)`xxxStorage.getItem('key')`;

   该方法接收一个键名作为参数，返回键名对应的值

   

   3)`xxxStorage.removeItem('key')`;

   该方法接收一个键名为参数，并把该键名从存储中删除。

   4)`xxxStorage.clear()`;

   该方法会清空存储中的所有数据

xxx:是local或者session



4. 备注：
   1. sessionStorage存储的内容会随着浏览器窗口关闭而消失。
   2. localStorage存储的内容，需要手动清除才会消失（浏览器关闭不会消失）
   3. `xxxStorage.getItem('key')`;如果对应的value值获取不到，返回的值是null。
   4. `JSON.parse(null)`返回的结果依然是null。

# session（不是sessionStorage）和cookie

session和cookie都是由服务器生成的，都是用来存储特定的值（键值对）

session是存储在服务器的，cookie是会返回给客户端的。SessionID是服务器用来识别、操作存储session值的对象的。

一般来说，在服务器端，session的存储方式有文件方式、数据库方式、而sessionID就是用来识别这个文件的（文件名相关）、识别数据库的某一条记录。**sessionID并不是session的值**

cookie一般是在客户端（浏览器）发送请求，会自动将存货、可用的cookie封装在请求头中和请求一起发送。

## session和cookie生命周期

1）cookie的生命周期一般来说受到两个因素影响

1. cookie自身的存活时间：是服务器生成cookie时设置的。
2. 客户端是否保留了cookie(**客户端能够自己存储cookie**)。客户端是否保留cookie，只对客户端自身有影响，对其他封包工具无影响

2）session的生命周期一般来说也受到俩因素影响

1. 服务器对session对象的保存的最大时间。
2. 客户端进程是否关闭。客户端进程关闭，也只对客户端自身有影响，对其他封包工具没有影响。

最后：**cookie和session都是有其作用域的**

