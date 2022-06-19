# noscript元素

`noscript`元素用来定义在脚本未执行时的替代文本，通常用于检查浏览器版本是否支持



代码：

```js
<body>  
...
  ...

  <script type="text/javascript">
    <!--
    document.write("Hello World!")
    //-->
  </script><noscript>Your browser does not support JavaScript!</noscript>...
  ...
</body> 
```

