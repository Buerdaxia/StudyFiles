# `React`中的`Diffing`算法

两个小问题：

1）`react/vue`中的`key`有什么作用？

2）为什么遍历列表时，`key`最好不要用`index`？



## 虚拟DOM中key的作用

首先简单说：`key`即使`DOM`对象的标识，在更新显示时，`key`有极大的作用。



详细：

首先：当状态中的数据发生变化时，`react`会更具【新数据】生成【新的虚拟`DOM`】，随后`react`进行【新虚拟`DOM`】与【旧虚拟`DOM`】的`diff`进行比较，比较的具体规则如下：

1. 旧虚拟DOM找到了与新虚拟`DOM`相同的`key`：

   （1）若虚拟DOM内容没变，直接使用之前的真实`DOM`

   （2）若虚拟DOM内容改变了，则生成新的真实`DOM`，随后替换掉页面中之前的真实`DOM`

2. 若没有找到相同的`key`：

   根据数据创建新真实`DOM`，最后渲染到页面上



## 用`index`作为`key`可能会应发的问题

1. 若对数据进行：逆序添加、逆序删除等破坏顺序性操作：

   会产生没有必要的真实`DOM`更新===>界面效果没问题，但是效率低

2. 如果结构中有输入型`DOM`：

   会产生错误`DOM`更新===>界面有问题

3. 注意：如果不存在对数据的逆序添加、逆序删除等破坏顺序型操作，仅仅用于渲染列表用于展示，使用`index`作为`key`还是没有问题的。



## 开发中如何选择key？

最好：使用每条数据唯一的标识作为`key`，如`id`、手机号、身份证号、学号等唯一标识。

一般：如果只是简单数据展示，用`index`也可以