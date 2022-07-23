# vue中各种修饰符

## 事件修饰符

1. prevent：可以阻止默认事件（常用）
2. stop：阻止事件冒泡（常用）
3. once：事件只触发一次（常用）
4. capture：使用事件的捕获模式(**捕获模式是，父元素和子元素绑定相同事件，触发子元素，会先触发父元素事件，再触发子元素事件和冒泡刚好相反**)
5. self：只有event.target是当前操作的元素时才触发事件(由于有事件冒泡机制，触发事件的可能不一定是绑定事件的元素本身)
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

注意：它们可以组合使用

示例：

```vue
<!--只有点击了button后才会触发，点击其他的不会触发-->
<button @click.self.prevent="handler">
  
</button>
```



## 键盘修饰符

* .enter：回车键
* .tab：tab制表符
* .delete：退格键
* .esc：ESC键
* .up：上键↑
* .down：下键↓
* .left：左键←
* .right：右键→



### 组合键修饰符

* .ctrl：ctrl键
* .alt：alt键
* .shift：shift键
* .meta：windows：徽标键，macos：Command键

组合按键就是要和别人组合在一起使用，自己单独使用不会生效。

还要注意一件事：组合按键是只要包含了我这两个按键，我就算你按了，例如`@keyup.ctrl.enter="xxx"`，但是用户只要按了`ctrl+enter`就会触发，按了`ctrl+alt+enter`也会触发

想要精确触发：使用修饰符`.exact`

示例：

```vue
<button @keup.ctrl.enter.exact="handler">
  监听ctrl+enter
</button>
```





## 鼠标修饰符

* .left：鼠标左键
* .middle：鼠标中间
* .right：鼠标右键



示例：

```vue
<button @mousedown.right="handler">
  监听鼠标右击
</button>
```



更多修饰符：可以查询mdn，但是注意一些组合键如`PageDown`在vue中要改成`@keyup.page-down`短横线表示法



## 表单输入相关修饰符

* .lazy
* .number
* .trim



`.lazy`示例：

```vue
<!-- 这种绑定方式，你输入1个字符，data中同步存储一个字符 -->
<input type="text" v-model="username" />

...
<script>
	data() {
		return {
      username: ''
    }
  }
</script>

<!--但是你加上.lazy之后，只有你输入字符后，然后input框失去焦点之后数据才会写入data中的username -->
<input type="text" v-model.lazy="username" />

...
<script>
	data() {
		return {
      username: ''
    }
  }
</script>
```





**`.number`修饰符**示例：

`vue`会底层调用`parseFloat()`方法来帮你转换字符串，如果不能转换，则原样返回字符串。

```vue
<!-- input输入框默认输入的是字符串，绑定到year里也是字符串 -->
<input type="text" v-model="year" />

...
<script>
	data() {
		return {
      year: ''
    }
  }
</script>

<!--但是你加上.number修饰符后，可以将输入的字符串转换为number类型 -->
<input type="text" v-model.number="year" />

<!-- 等同于-->
<input type="number" v-model="year" />
...
<script>
	data() {
		return {
     	year: ''
    }
  }
</script>
```





`.trim`修饰符：

可以去除用户输入内容的首位空格

```vue
<input type="text" v-model.trim="value" />
```

