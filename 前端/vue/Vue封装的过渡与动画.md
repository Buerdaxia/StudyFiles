# Vue封装的过渡与动画

**前提：组件|元素务必要拥有`v-show`或`v-if`的指令，或者组件切换（因为组件切换会销毁和产生）**

1）作用：在插入、更新或者移动DOM元素时，在合适的时候给元素添加样式类名。

2）图示：

![vue过渡动画](C:/Users/UM0103677/Desktop/笔记/StudyFiles/前端图片/vue/vue过渡动画.png)

3）写法

（1）准备好样式

元素进入时的样式：

* v-enter：进入的起点
* v-enter-active：进入过程中
* v-enter-to：进入的终点

元素离开时的样式：

* v-leave：离开的起点
* v-leave-active：离开过程中
* v-leave-to：离开的终点

（2）使用`<transition>`包裹要过渡的元素，并配置name属性：

```html
<transition name="hello">
	<h1 v-show="isShow">你好啊！</h1>
</transition>
```

（3）备注：若有多个元素需要过渡，则需要使用：`<transition-group>`,并且每个元素都要指定`key`值



## 使用

情况一：如果没有给`transition`绑定`name`属性，则使用默认的v-enter...

示例

```vue
<transition>
	<h1 v-show="isShow">你好啊！</h1>
</transition>

<style>
    // 进入时的三组
    .v-enter {
        opcity: 0;
    }
    .v-enter-active {
        transition: .5s;
    }
    .v-enter-to {
        opcity: 1;
    }
    // 离开动画的三组
    .v-leave {
        opcity: 1;
    }
    .v-leave-active {
        transition: .5s;
    }
    .v-leave-to {
        opcity: 0;
    }
</style>
```



第二种情况如果给`transition`标签添加了`name`属性，则需要修改以下类名

```vue
<transition name="fade">
	<h1 v-show="isShow">你好啊！</h1>
</transition>

<style>
    // 需要修改以下v-
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
    
.fade-enter, .fade-leave-to 
  opacity: 0;
}
</style>
```

## 过渡动画小问题

在使用组件过渡动画时，在上一个组件没有完全过渡完毕时，下一个切换组件就会进入过渡动画。

这种进入和离开同时生效有时会造成卡顿问题。

解决方式：

Vue提供了三种过渡模式（Vue我的超人！）

* **in-out**：新元素先进行过渡，完成之后当前元素过渡离开
* **out-in**：当前元素先进行过渡，完成之后新元素再过渡进入

使用方法：

在`transition`标签新添加一个`mode`属性即可

```vue
<transition name="fade" mode="out-in">
  <!-- ... 要被添加动画的元素 ... -->
</transition>
```

