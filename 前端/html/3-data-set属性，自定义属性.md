# data-set属性，自定义属性

这个可以用来存储一些需要自己保存的数据

## data-xxx属性设置方法

还有一种特殊的设置方式，直接写进`html`标签中

```html
<!-- 其中前面部分是固定的data后面可以修改-->
<audio data-key="71" src="xxx"></audio>
```



`dataset`**设置方法**有两种，一种通过方法`setAttribute()`方法设置

方法一：

```js
let img = document.querySelector('img');
img.setAttribute('data-src', 'img.jpg');
```



方法二：直接调`dataset`的`API`

```JS
let img = document.querySelector('img');
img.dataset.src = 'img.jpg';

// 这两种方法都设置出来
// <img data-src="img.jpg"/>
```

## dataset属性获取方法

`dataset`的**获取方法**也有两种.

方法一：`getAttribute()`

```js
let img = document.querySelector('img');
img.src = img.getAttribute('data-src')
```

方法二：直接获取

```js
let img = document.querySelector('img');
img.src = img.dataset.src
```

