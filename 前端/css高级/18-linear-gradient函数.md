# linear-gradient函数

MDN:CSS **`linear-gradient()`** 函数用于创建一个表示两种或多种颜色线性渐变的图片。



基本写法：

```css
/* 渐变轴为45度，从蓝色渐变到红色 */
linear-gradient(45deg, blue, red);

/* 从右下到左上、从蓝色渐变到红色 */
linear-gradient(to left top, blue, red);

/* 从下到上，从蓝色开始渐变、到高度 40% 位置是绿色渐变开始、最后以红色结束 */
linear-gradient(0deg, blue, green 40%, red);
```



注意：**该函数是写在`background-image属性中的`**

示例：

```css
background-image: linear-gradient(to left top, blue, red);

/*也可以写在简写属性中，但是它核心就是写在bgi中*/
background: linear-gradient(to left top, blue, red);
```



## 给一个背景图片添加渐变效果

核心：`background-image`属性是可以添加两个值的，第一个我们可以写渐变颜色，第二个我们可以引入一张图片，从而让图片有渐变效果

注意：**默认第一个会参数，盖住第二个**

```css
.section-features {
  padding: 20rem 0;
  /*这里第一个渐变，第二个图片url用逗号隔开*/
  background-image: linear-gradient(
			to right bottom,
			rgba($color-primary-light, 0.8), /*设置了透明度*/
			rgba($color-primary-dark, 0.8)
		),
		url(../img/nat-4.jpg);
  background-size: cover;
}
```





## linear-gradient更高级的用法

我们可以为每一种颜色指定一个范围（图片中渐变的范围）

可以自己写一个`div`看一下效果

示例：

```scss
.book {
  background-image: linear-gradient(105deg,
    rgba(255,255,255, .9) 50%,	// 在颜色后面跟一个百分比来指定范围
    orangered 50%),
    url(../img/nat-10.jpg);
  background-size: cover;
}

// 上面这种写法的意思就是
// 第一个105deg向105deg方向进行渐变
// 第二个:白色占到50%
// 第三个：橘色orangered占到50%


```



## 制作渐变字体

注意，还需要使用一个比较新的css属性是：`background-clip`，可以看上一篇md文档

示例：

```scss
$color-primary-light: #55c57a;
$color-primary-dark: #28b485;
.heading-secondary {
  // 注意：这里最好也设置成inline-block让字体撑开元素
  display: inline-block;
  font-size: 3.5rem;
  // 1.先设置一个渐变的背景颜色
  background-image: linear-gradient(to right, $color-primary-light, $color-primary-dark);
  // 2.使用clip属性，让背景只显示到文字的位置上
  -webkit-background-clip: text;
  // 3.让字体透明
  color: transparent;
  letter-spacing: 2px;
  transition: all 0.2s;
}

// 这样3步设置下来，就能够得到一个有渐变特效的字体了
```

