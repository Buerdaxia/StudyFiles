# fontawesome的使用

1. 下载fontawesome
2. 解压
3. 将css和webfonts文件夹移动到项目中（同一级目录下）
4. 将all.css引入网页中
5. 使用图标字体



使用方式：

1）直接通过类来使用图标字体

```html
<i class="fas xxx"></i>
<i class="fab xxx"></i>

fas和fab是必写的
xxx是图标样式名儿
```

2）通过编码设置(在css文件中设置)

```css
li::before {
	content: '\xxxx';
	font-family: '在all.css中查询下fab或者fas的字体';
	font-weight: ;可能有，可能没有
}
```

例子：

```css
li::before {
	content: '\f1b0';
    /*font-family: 'Font Awesome 5 Brands';*/
	font-family: 'Font Awesome 5 Free';
	font-weight: 900;
}

这样li标签前面都会有编码是f1b0的图片了
还能设置别的样式例如颜色大小等
```



3）通过实体&#x图标编码

```css
<i class="fas">&#xf0f3</i>

<i class="fab">&#x编码</i>
```

