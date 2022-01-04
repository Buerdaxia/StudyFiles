# Lodash使用

但是有时候，安装别的插件时，别的插件可能已经引入了`loadsh`，我们在`node_modules`中搜索一下`lodash`即可，如果有就不用安装了。

官网下载。

```
//vue中 npm 一下
npm install -s lodash
```



lodash是以`_`作为开头的，和jq的`$`一样

## 全部引入

![lodash全部引入](C:\Users\UM0103677\Desktop\笔记\StudyFiles\前端图片\lodash\lodash全部引入.PNG)

使用时只需要：

```
_.函数名()
//节流函数
_.throttle()
```



## 按需引入

![lodash按需引入](../../前端图片/lodash/lodash按需引入.PNG)

只需要引入自己需要的函数即可

使用时：**将原来的es6简写属性，展开成es5的时候**

![节流函数使用](../../前端图片/lodash/节流函数使用.PNG)





## 使用注意

## **使用`lodash`的函数时，尽量不要使用箭头函数，容易导致`this`的指向出现问题。**

