# TodoList案例总结

1. 拆分组件、实现静态组件。注意className、style的写法

2. 动态初始化列表，如何确定数据放在那个组件的state中？

   —某个组件使用：就放在其自己身上的state

   —某些组件使用：就放在他们共同的父组件的state中（官方称此操作为：状态提升）

3. 关于父子之间的通讯：

   —父给子，通过props传递

   —子给父，通过props传递一个函数，子组件调用

4. 注意defaultChecked和checked的区别，类似的还有defaultValue与value

5. 状态在哪里，操作状态的方法就在哪里



还有：

操作数组时，reduce与filter的使用，还有map的使用

reduce可以求和，filter用作删除，map用来修改