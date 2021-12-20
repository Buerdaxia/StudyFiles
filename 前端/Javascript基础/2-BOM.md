# BOM(browser object module)

浏览器对象模型

BOM可以让我们通过JS来操作浏览器，在BOM中位我们提供了一组对象，用来完成对浏览器的操作。

## BOM 对象

1. Window
2. Navigator
3. Location
4. History
5. Screen

## window对象

window代表的使整个浏览器的窗口，同时window也是网页中的去全局对象。

## Navigator对象

navigator代表的使当前浏览器的信息，通过该对象可以来识别不同的浏览器

其实由于历史原因，navigator的大部分实现都已经不能帮我们识别浏览器了

现在一般用**userAgent**来判断浏览器的信息。

navigator.userAgent 会返回一串字符串，字符串里放着浏览器的一些信息。

## Location对象

location代表当前浏览器的地址栏，通过location可以获取地址栏信息或者操作跳转页面。

该对象封装了浏览器的地址栏信息。

## History对象

history代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录。

注意：由于隐私原有，该对象不能获取到具体的历史记录，只能操作浏览器向前或者向后。而且该操作只能在当此访问时有效（一层只能访问一个）。

## Screen对象

screen代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关信息。



## 使用方法

上面的这些对象都是作为window对象的属性保存的，可以用window对象来进行使用。

