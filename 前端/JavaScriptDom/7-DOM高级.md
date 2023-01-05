# DOM高级



##  MutationObserver 接口

作用：到 DOM 规范中的 MutationObserver 接口，可以在 DOM 被修改时异步执行回调。使 用 MutationObserver 可以观察整个文档、DOM 树的一部分，或某个元素。此外还可以观察元素属性、子节点、文本，或者前三者任意组合的变化。



基本语法：

```js
let observer = new MutationObserver(() => console.log('DOM was mutated!'));
// 需要调用构造函数来创建实例
```



### 实例方法observe()

新创建的 MutationObserver 实例不会关联 DOM 的任何部分。要把这个 observer 与 DOM 关 联起来，需要使用 observe()方法。这个方法接收两个必需的参数：要观察其变化的 DOM 节点，以及 一个 MutationObserverInit 对象。



示例：

```js
let observer = new MutationObserver(() => console.log('<body> attributes changed')); 
observer.observe(document.body, { attributes: true });
// 观测body身上的属性，一旦发生变化就会调用构造函数中传如的方法

document.body.className = 'foo'; 
console.log('Changed body class'); 
// Changed body class 
// <body> attributes changed 
```



>更加具体的使用方式与介绍，请查看《JavaScript高级程序设计》 p-458



##  回调与 MutationRecord（回调参数一）

构造函数传入的回调，会有一个参数是一个收集了叫做MutationRecord 实例的**数组**，MutationRecord实例，记录着每次发生的变化。



示例：

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords));
observer.observe(document.body, { attributes: true }); 
document.body.setAttribute('foo', 'bar'); 

// [ 
// { 
// addedNodes: NodeList [], 
// attributeName: "foo", 
// attributeNamespace: null, 
// nextSibling: null, 
// oldValue: null, 
// previousSibling: null 
// removedNodes: NodeList [], 
// target: body 
// type: "attributes" 
// } 
// ]


// 发生多次改变，会传如多个record
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { attributes: true }); 
document.body.className = 'foo'; 
document.body.className = 'bar'; 
document.body.className = 'baz'; 
// [MutationRecord, MutationRecord, MutationRecord] 
```



实例属性代表的含义：

| 属性               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| target             | 被修改影响的目标节点                                         |
| type               | 字符串，表示变化的类型："attributes"、"characterData"或"childList" |
| oldValue           | 如果在 MutationObserverInit 对象中启用（attributeOldValue 或 characterData OldValue 为 true），"attributes"或"characterData"的变化事件会设置这个属性为被替代的值 "childList"类型的变化始终将这个属性设置为 null |
| attributeName      | 对于"attributes"类型的变化，这里保存被修改属性的名字 其他变化事件会将这个属性设置为 null |
| attributeNamespace | 对于使用了命名空间的"attributes"类型的变化，这里保存被修改属性的名字 其他变化事件会将这个属性设置为 null |
| addedNodes         | 对于"childList"类型的变化，返回包含变化中添加节点的 NodeList 默认为空 NodeList |
| removedNodes       | 对于"childList"类型的变化，返回包含变化中删除节点的 NodeList 默认为空 NodeList |
| previousSibling    | 对于"childList"类型的变化，返回变化节点的前一个同胞 Node  默认为 null |
| nextSibling        | 对于"childList"类型的变化，返回变化节点的后一个同胞 Node 默认为 null |



## 回调参数二

构造中传如的回调的第二个参数，是观察变化的 MutationObserver 的实例，

示例：

```js
let observer = new MutationObserver( 
 (mutationRecords, mutationObserver) => console.log(mutationRecords,
mutationObserver)); 
observer.observe(document.body, { attributes: true }); 
document.body.className = 'foo'; 
// [MutationRecord], MutationObserver 

```





##  disconnect()方法

默认情况下，只要被观察的元素不被垃圾回收，MutationObserver 的回调就会响应 DOM 变化事 件，从而被执行。要提前终止执行回调，可以调用 disconnect()方法。下面的例子演示了同步调用 disconnect()之后，**不仅会停止此后变化事件的回调**，也会**抛弃已经加入任务队列要异步执行的回调**：

简单来说：就是会抛弃所有observer监听的异步回调



示例：

```js
let observer = new MutationObserver(() => console.log('<body> attributes changed')); 
observer.observe(document.body, { attributes: true }); 
document.body.className = 'foo'; 
observer.disconnect(); 
document.body.className = 'bar'; 
//（没有日志输出）
```



如果想要让异步执行完毕再抛弃，可以把disconnect()放到setTimeout中：

```js
let observer = new MutationObserver(() => console.log('<body> attributes changed')); 
observer.observe(document.body, { attributes: true }); 
document.body.className = 'foo'; 
setTimeout(() => { 
 observer.disconnect(); 
 document.body.className = 'bar'; 
}, 0); 
// <body> attributes changed 
```

>原因：因为定时器也是异步的，他会和上面的异步任务一起假如异步队列中，上面的会先调用，之后disconnect()再会调用



## 复用 MutationObserver

复用最主要的关键，就是需要一个标识，不同传如的内容，要有不同的标识标志着。刚好MutationRecord的target属性，就是标志发生变化的节点，不同节点target属性名称也会不同



示例：复用一个MutationObserver观察多个节点

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords.map((x) => 
x.target))); 
// 向页面主体添加两个子节点
let childA = document.createElement('div'), 
 childB = document.createElement('span'); 
document.body.appendChild(childA); 
document.body.appendChild(childB); 
// 观察两个子节点
observer.observe(childA, { attributes: true }); 
observer.observe(childB, { attributes: true }); 
// 修改两个子节点的属性
childA.setAttribute('foo', 'bar'); 
childB.setAttribute('foo', 'bar'); 
// [<div>, <span>] 



// 如果调用了disconnect()方法，则会直接清空所有MutationObserver加入的异步任务
```





##  MutationObserverInit 与观察范围

MutationObserverInit 对象用于控制对目标节点的观察范围。粗略地讲，观察者可以观察的事 件包括**属性变化、文本变化和子节点变化**。



具体如下面的表格：

| 属性                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| subtree               | 布尔值，表示除了目标节点，是否观察目标节点的子树（后代） 如果是 false，则只观察目标节点的变化；如果是 true，则观察目标节点及其整个子树 默认为 false |
| attributes            | 布尔值，表示是否观察目标节点的属性变化 默认为 false          |
| attributeFilter       | 字符串数组，表示要观察哪些属性的变化 把这个值设置为 true 也会将 attributes 的值转换为 true  默认为观察所有属性 |
| attributeOldValue     | 布尔值，表示 MutationRecord 是否记录变化之前的属性值 把这个值设置为 true 也会将 attributes 的值转换为 true  默认为 false |
| characterData         | 布尔值，表示修改字符数据是否触发变化事件 默认为 false        |
| characterDataOldValue | 布尔值，表示 MutationRecord 是否记录变化之前的字符数据 把这个值设置为 true 也会将 characterData 的值转换为 true  默认为 false |
| childList             | 布尔值，表示修改目标节点的子节点是否触发变化事件 默认为 false |



>注意： 在调用 observe()时，MutationObserverInit 对象中的 attribute、characterData 和 childList 属性必须至少有一项为 true（无论是直接设置这几个属性，还是通过设置 attributeOldValue 等属性间接导致它们的值转换为 true）。否则会抛出错误，因为没 有任何变化事件可能触发回调。





### 观察属性

MutationObserver 可以观察节点属性的添加、移除和修改。要为属性变化注册回调，需要在 MutationObserverInit 对象中将 attributes 属性设置为 true。

示例：

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { attributes: true }); 
// 添加属性 
document.body.setAttribute('foo', 'bar'); 
// 修改属性
document.body.setAttribute('foo', 'baz'); 
// 移除属性
document.body.removeAttribute('foo'); 
```



如果想指定观察那些属性有变化，可以利用`attributeFilter `属性来设置白名单，即 一个属性名字符串数组

示例：

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { attributeFilter: ['foo'] }); 
// 添加白名单属性
document.body.setAttribute('foo', 'bar'); 
// 添加被排除的属性
document.body.setAttribute('baz', 'qux'); 
```



**如果还想再变化中记录保存的值，还可以将attributeOldValue 属性设置为 true**

示例：

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords.map((x) => x.oldValue))); 
observer.observe(document.body, { attributeOldValue: true }); 
document.body.setAttribute('foo', 'bar'); 
document.body.setAttribute('foo', 'baz'); 
document.body.setAttribute('foo', 'qux'); 
// 每次变化都保留了上一次的值
// [null, 'bar', 'baz'] 
```



### 观察字符数据

MutationObserver 可以观察文本节点（如 Text、Comment 或 ProcessingInstruction 节点） 中字符的添加、删除和修改。要为字符数据注册回调，需要在 MutationObserverInit 对象中将 characterData 属性设置为 true

**具体的操作，其实和上面的观察属性差不多，可以看上面的，然后对照的属性表格**







### 观察子节点

MutationObserver 可以观察目标节点子节点的添加和移除。要观察子节点，需要在 MutationObserverInit 对象中将 childList 属性设置为 true。

示例：

```js
// 清空主体
document.body.innerHTML = ''; 
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { childList: true }); 
document.body.appendChild(document.createElement('div')); 
// [ 
// { 
// addedNodes: NodeList[div], 
// attributeName: null, 
// attributeNamespace: null, 
// oldValue: null, 
// nextSibling: null, 
// previousSibling: null, 
// removedNodes: NodeList[], 
// target: body, 
// type: "childList", 
// } 
// ] 



// 移除子节点
// 清空主体
document.body.innerHTML = ''; 
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { childList: true }); 
document.body.appendChild(document.createElement('div')); 
// [ 
// { 
// addedNodes: NodeList[], 
// attributeName: null, 
// attributeNamespace: null, 
// oldValue: null, 
// nextSibling: null, 
// previousSibling: null, 
// removedNodes: NodeList[div], 
// target: body, 
// type: "childList", 
// } 
// ] 
```





### 观察子树

默认情况下，MutationObserver 将观察的范围限定为一个元素及其子节点的变化。可以把观察 的范围扩展到这个元素的子树（所有后代节点），这需要在 MutationObserverInit 对象中将 subtree 属性设置为 true。

示例：

```js
// 清空主体
document.body.innerHTML = ''; 
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
// 创建一个后代
document.body.appendChild(document.createElement('div'));
/ 观察<body>元素及其子树
observer.observe(document.body, { attributes: true, subtree: true }); 
// 修改<body>元素的子树
document.body.firstChild.setAttribute('foo', 'bar'); 
// 记录了子树变化的事件
// [ 
// { 
// addedNodes: NodeList[], 
// attributeName: "foo", 
// attributeNamespace: null, 
// oldValue: null, 
// nextSibling: null, 
// previousSibling: null, 
// removedNodes: NodeList[], 
// target: div, 
// type: "attributes", 
// } 
// ] 
```



有意思的是，被观察子树中的节点被移出子树之后仍然能够触发变化事件。这意味着在子树中的节 点离开该子树后，即使严格来讲该节点已经脱离了原来的子树，但它仍然会触发变化事件。

示例：

```js
// 清空主体
document.body.innerHTML = ''; 
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
let subtreeRoot = document.createElement('div'), 
 subtreeLeaf = document.createElement('span'); 
// 创建包含两层的子树
document.body.appendChild(subtreeRoot); 
subtreeRoot.appendChild(subtreeLeaf); 
// 观察子树
observer.observe(subtreeRoot, { attributes: true, subtree: true }); 
// 把节点转移到其他子树
document.body.insertBefore(subtreeLeaf, subtreeRoot); 
subtreeLeaf.setAttribute('foo', 'bar'); 
// 移出的节点仍然触发变化事件
// [MutationRecord] 
```





## 异步回调与记录队列

MutationObserver 接口是出于性能考虑而设计的，其核心是异步回调与记录队列模型。**为了在 大量变化事件发生时不影响性能，每次变化的信息（由观察者实例决定）会保存在 MutationRecord 实例中，然后添加到记录队列**。这个队列对每个 MutationObserver 实例都是**唯一**的，是所有 DOM 变化事件的有序列表。



### 记录队列

每次 MutationRecord 被添加到 MutationObserver 的记录队列时，仅当之前没有已排期的微任 务回调时（队列中微任务长度为 0），才会将观察者注册的回调（在初始化 MutationObserver 时传入） 作为微任务调度到任务队列上。这样可以保证记录队列的内容不会被回调处理两次。 不过在回调的微任务异步执行期间，有可能又会发生更多变化事件。因此被调用的回调会接收到一 个 MutationRecord 实例的数组，顺序为它们进入记录队列的顺序。回调要负责处理这个数组的每一 个实例，因为函数退出之后这些实现就不存在了。回调执行后，这些 MutationRecord 就用不着了， 因此记录队列会被清空，其内容会被丢弃。



### takeRecords()方法

调用 MutationObserver 实例的 takeRecords()方法可以清空记录队列，取出并返回其中的所 有 MutationRecord 实例。

示例：

```js
let observer = new MutationObserver( 
 (mutationRecords) => console.log(mutationRecords)); 
observer.observe(document.body, { attributes: true }); 
document.body.className = 'foo'; 
document.body.className = 'bar'; 
document.body.className = 'baz'; 
console.log(observer.takeRecords()); 
console.log(observer.takeRecords()); 
// [MutationRecord, MutationRecord, MutationRecord] 
// [] 
```



**这在希望断开与观察目标的联系，但又希望处理由于调用 disconnect()而被抛弃的记录队列中的 MutationRecord 实例时比较有用。**