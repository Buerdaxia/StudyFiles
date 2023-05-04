# uni中使用animate.css不生效问题

将`animate.css`中的:

```css
:root {
  --animate-duration: 1s;
  --animate-delay: 1s;
  --animate-repeat: 1;
}
```



修改为：

```css
page {
  --animate-duration: 1s;
  --animate-delay: 1s;
  --animate-repeat: 1;
}
```

原因：小程序，app中，根元素是page不是这个:root



# 如何在uni-app中使用animate.css

1. 首先，可以先到官网上将`animate.css`的css文件下载下来
2. 按照上面不生效问题，修改一下名称
3. 在`App.vue`中的style中，直接将`animate.css`引入进来即可

```vue
<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "uview-ui/index.scss";
  // 图标库
  @import "common/icon.css"; 
  // 全局样式
  @import "common/common.css";
  // 动画库
  @import "common/animate.css";
  // ui样式库
  @import "common/uni-main.css"
</style>
```

4. 可以到官网上查询每个css类怎么使用（官网有很多中使用方式自行查询）

示例：(使用淡入动画（是通过类的方式来使用的）)

```vue
<view class="animate__animated animate__fadeIn animate__faster" style="background: #f5f5f5;">
</view>
```

