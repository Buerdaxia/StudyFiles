# 样式问题

如果需要修改组件的样式。

两种方案：

一、去掉组件样式中的`scoped`限制，但是会污染全局环境。





二、利用穿透，穿透scoped `/deep/`或者`>>>`



```css
<style lang="sass" scoped>

/deep/ 类名 {
	样式xxxx
}

// sass可能不认识 >>> 这个符号，建议还是写/deep/
外层 >>> 第三方组件{
	样式xxxx
} 

</style>
```

