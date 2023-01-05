# @supports关键字优雅降级

其实这个以前用的比较多，现在其实用的就比较少了。

关键字的功能：它就是可以帮助我们检查一个属性在当前运行的浏览器中是否支持，如果支持就执行`{...}`中的代码。



直接看待代码吧：

```scss
// 这句话的意思是，如果浏览器支持backdrop-filter属性或这加上-webkit-时支持，那么就添加上backdrop-filter: blur(10px);这个css样式
.heading {
  
  @supports (backdrop-filter: blur(10px)) or (-webkit-backdrop-filter: blur(10px)) {
    backdrop-filter: blur(10px);
  }
}
```

