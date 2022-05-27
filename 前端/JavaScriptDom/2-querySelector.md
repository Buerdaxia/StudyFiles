# querySelector

**使用这个属性需要注意兼容性，如果要适配ie就要小心了**

**注意**：利用`querySelector`取到`dom`元素数量是静态的，就是加入页面上有10个`dom`，如果突然动态添加了一个`dom`变为11,除非重新获取，否则querySelector获得仍然是10个，

getElement系列是获取的动态的dom

## document.querySelector()

需要一个选择器的字符串作为参数，可以根据一个css选择器来查询一个元素节点对象！（好用的呀匹）

注意：只能获取1个，如果没有返回null

>特殊用法：

需要传递一个字符串，但是再传递标签选择器时，能够指定属性来选择单个通过`[]`将属性包裹

```html
<div data-key="10" ></div>
<div data-key="20" ></div>
// 选中单独的key为10的div
<script>
	const d = document.querySelector('div[data-key="10"]');
</script>
```









## document.querySelectorAll()

上面的孪生进版，可以获取一组的符合条件的标签，需要传如一个选择器字符串

注意：只有一个符合条件的元素也是返回一个类数组



