## CSS水平绘制居中的布局

## 1、定宽高

### 一、绝对定位+负magin值

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    position: relative;
			}
			.children-box {
				position: absolute;
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
				left: 50%;
				top: 50%;
				margin-left: -50px;
				margin-top: -50px;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				
			</div>
		</div>
	</body>
</html>

```

### 二、绝对定位+transform

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    position: relative;
			}
			.children-box {
				position: absolute;
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
				left: 50%;
				top: 50%;
				transform: translate(-50%,-50%);
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				
			</div>
		</div>
	</body>
</html>

```

### 三、绝对定位加left、right、top、bottom为0加margin

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
		.box {
		    width: 200px;
		    height: 200px;
		    border: 1px solid red;
		    position: relative;
		}
		.children-box {
		    position: absolute;
		    display: inline;
		    top: 0;
		    left: 0;
		    right: 0;
		    bottom: 0;
		    background: yellow;
		    margin: auto;
		    height: 100px;
		    width: 100px;
		}
		</style>
	</head>
	<body>
		
		    <div id="app">
		        <div class="box">
		            <div class="children-box"></div>
		        </div>
		    </div>

	</body>
</html>

```

### 四、flex布局

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    display: flex;
				justify-content: center;
				align-items: center;
			}
			.children-box {
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				
			</div>
		</div>
	</body>
</html>

```

### 五、grid布局

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    display: grid;
			}
			.children-box {
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
				margin: auto;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				
			</div>
		</div>
	</body>
</html>

```

### 六、容器配置table-cell+vertical-align+text-align+要居中元素必须为inline-block

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    display: table-cell;
				vertical-align: middle;
				text-align: center;
			}
			.children-box {
				display: inline-block;
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				
			</div>
		</div>
	</body>
</html>

```

## 2、不定宽高

### 一、绝对定位+transform

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    position: relative;
			}
			.children-box {
				position: absolute;
				background-color: yellowgreen;
				left: 50%;
				top: 50%;
				transform: translate(-50%,-50%);
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				坚持不懈
			</div>
		</div>
	</body>
</html>

```

### 二、table-cell

table-cell能这样实现居中的一个原有就是，表格内容是线对齐的

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
				display: table-cell;
				vertical-align: middle;
				text-align: center;
			}
			.children-box {
				display: inline-block;
				background-color: yellowgreen;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				坚持不懈
			</div>
		</div>
	</body>
</html>

```

### 三、flex布局

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    display: flex;
				justify-content: center;
				align-items: center;
			}
			.children-box {
				background-color: yellowgreen;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				锲而不舍
			</div>
		</div>
	</body>
</html>

```

### 四、grid+margin布局

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.box {
			    width: 200px;
			    height: 200px;
			    border: 1px solid red;
			    display: grid;
			}
			.children-box {
				background-color: yellowgreen;
				margin: auto;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<div class="children-box">
				持之以恒
			</div>
		</div>
	</body>
</html>

```

# 总结

## 一、内联元素居中布局

1）水平居中：

行内元素可设置：text-align:center;

flex布局设置父元素：display:flex;justify-content:center;

2）垂直居中：

单行文本元素确认高度：height === line-height

多行文本父元素确认高度：display: table-cell;加上vertical-align:middle;

## 二、块级元素居中布局

1）水平居中：

定宽：margin：0 auto；

不定宽：参考上述不定宽高的例子

2）垂直居中：

一、(定高) position:absolute；设置left，top,margin-left和margin-top；

二、transform:translate(x,y);配合绝对定位加left+top等(全能)

三、flex（全能）

四、grid（全能）

五、table-cell（全能）

