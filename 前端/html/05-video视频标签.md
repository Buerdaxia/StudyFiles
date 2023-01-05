# video视频标签

作用：**HTML `<video>` 元素** 用于在 HTML 或者 XHTML 文档中嵌入媒体播放器，用于支持文档内的视频播放。你也可以将 `<video>` 标签用于音频内容，但是 `<audio>`) 元素可能在用户体验上更合适。（来自MDN）



这个标签有几个自带的属性：

1. `muted`：布尔属性，指明在视频中音频的默认设置。设置后，音频会初始化为静音。默认值是 false, 意味着视频播放的时候音频也会播放。
2. `loop`：布尔属性；指定后，会在视频播放结束的时候，自动返回视频开始的地方，继续播放。
3. `autoplay`：布尔属性；声明该属性后，视频会尽快自动开始播放，不会停下来等待数据全部加载完成。

其他属性请查看MDN



他经常和另一个标签配合使用`<source>`，它是用来引入资源的，类似于`src`属性

示例：

```html
 <!--视频自动播放，静音，循环-->
<video class="bg-video__content" autoplay muted loop>
  <!--这两个是用来适配浏览器的-->
  <source src="img/video.mp4" type="video/mp4">
  <source src="img/video.webm" type="video/webm">
  
  
	<!--上面两个都加载不出来时，机会显示这句话-->
  Your browser is not supported!
</video>
```

