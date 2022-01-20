# el-button修改图标

如果不想使用element-ui中自带的图标，想要自定义图标。



第一步：

肯定先创建一个`el-button`出来

```vue
<el-button size="mini"
           type="primary"
           class="default"
           icon="el-icon-my-debug"
           @click="handleDebug(scope)"></el-button>
```

其中icon是我们自己定义的。

第二步：

将找到的图片丢进项目中，并创建如下css样式

```vue
<style lang="less" scoped>
// 定义调试图标
  /deep/.el-icon-my-debug {
    background: url("../../assets/images/tiaoshi.png") center no-repeat;
    background-size: cover;
  }
  /deep/.el-icon-my-debug:before {
    content: "替";
    font-size: 16px;
    visibility: hidden;
    // 这个是隐藏替字
  }
</style>
```

注意：如果style中有scoped属性一定要在类前面加上`/deep/`样式穿透，否则图标不会显示