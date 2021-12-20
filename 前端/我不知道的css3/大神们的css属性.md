# 大神们的css属性

## column-gap

很像grid布局中的grid-column-gap，能够指定列间距一般和column-count一起使用

## column-count

能够将div元素中的内容按指定数值分开

```css
div {column-count: 3;} /*将div内容分为三栏*/
```

## 滚动贴合特效二人组

### scroll-snap-type（放在父元素上）

```css
scroll-snap-type: 参数一, 参数二
```

参数一：表示滚动贴合方向可选值-->x或y

参数二：表示贴合方式可选值

* mandatory：表示强制贴合，用户一滚动鼠标就贴合下一屏内容
* proximity：表示当滚动距离小于一定值后，再进行贴合(**用于滚动内容很长的时候**)

### scroll-snap-aligin(放在子元素上)

此属性表示贴合内容和scroll-snap-type一起使用

```css
scroll-snap-aligin: start; /*顶部贴合*/
scroll-snap-aligin: end; /*底部贴合*/
scroll-snap-align: center; /*居中贴合，里面的内容会居中*/
```

## scroll-padding

规定滚动内容距离的边距：和padding取值一样上右下左

```css
    h2 {
      /* 如果上面有标题 就会将滚动的一部分内容给遮住 */
      position: sticky;
      top: 0;
      height: 80px;
      background-color: #fff;
      text-align: center;
      line-height: 80px;
    }

    main {
      /* 第一个参数滚动方向， */
      scroll-snap-type: y mandatory;
      /* 解决滚动内容被上面h2遮住的问题 */
      scroll-padding: 80px;
      overflow: scroll;
      height: 100vh;
    }
```



## css计数器

CSS 计数器使用到以下几个属性：

- `counter-reset` - 创建或者重置计数器
- `counter-increment` - 递增变量
- `content` - 插入生成的内容（说明计数器出现在before或者after中）
- `counter(name)` 或 `counters(name)` 函数 - 将计数器的值添加到元素

```css
/*左上角有个计数小图标*/
.item {
  padding: 2px;
  position: relative;
  counter-increment: item-counter;
}
.item::after {
  position: absolute;
  display: block;
  top: 2px;
  left: 2px;
  width: 24px;
  height: 24px;
  text-align: center;
  line-height: 24px;
  background-color: #000;
  color: #fff;
  content: counter(item-counter);
}
```

## calc计算函数

calc() 函数用于动态计算长度值。

- 需要注意的是，运算符前后都需要保留一个空格，例如：`width: calc(100% - 10px)`；
- 任何长度值都可以使用calc()函数进行计算；
- calc()函数支持 "+", "-", "*", "/" 运算；
- calc()函数使用标准的数学运算优先级规则；

关键用法，查mdn

