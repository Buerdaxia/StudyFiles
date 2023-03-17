# uni中使用富文本解析后端数据

很多资讯页面，例如淘宝里面最下面的商品详情，或者是一段新闻内容的详情页面，后端都会选择返回markdown字符串或者html字符串，这时候我们就可以用一些富文本解析器来**直接解析出来**，放在页面上就可以了。



富文本解析的组件有



## rich-text（uni官方自带的）

第一种uni官方自己的一个组件，可以直接使用的



>简单使用

```vue
<rich-text :nodes="string"></rich-text>

<script>
export default {
    data() {
        return {
            strings: '<div style="text-align:center;"><img src="https://web-assets.dcloud.net.cn/unidoc/zh/uni@2x.png"/></div>'
        }
    }
}
</script>
```





## uParse插件（需要去DCLOUD插件市场下载）

这个组件也比较方便，自己带了很多的方法，使用起来和上面的差不多，自己去插件市场搜一搜就好了

>其实：uni-ui中就有这个插件，如果用uni-ui了可以去到里面找一找

示例：

```vue
<template>
	<uParse :content="article" @preview="preview" @navigate="navigate"></uParse>
	<!-- @preview是在富文本中触发了预览时间时会触发， navigate是富文本中触发了跳转事件时欻navigate -->
</template>

<script>
	export default {
    data() {
      return {
				article: '<p>我是html字符串</p>'      
    	}
    },
    methods: {
      preview(src, e) {
        // 触发图片预览
      },
      navigate(href, e) {
        // 允许点击超链接，点击后这里就会触发
        console.log('点击了超链接, 链接为:', href);
      }
    }
  }
</script>
```

