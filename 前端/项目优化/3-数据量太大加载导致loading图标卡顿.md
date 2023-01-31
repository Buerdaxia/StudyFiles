# 数据量太大加载导致loading图标卡顿

假设后台一下返回10000条数据，如果我们一次性把它放到页面上，一下子渲染不过来，所以会卡顿。

如果不考虑虚拟列表，我们的优化思路就是，让渲染线程不要一下子那么忙，请求 js 老兄协助把数组分割一下，然后把数据一段一段的显示到页面，可利用  window.requestAnimationFrame或者setInterval不断填充数据；

window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在 下次重绘之前 调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行；



核心函数：

```js

// data为所有数据的数组 ， callback 拿到当数据要做的操作 ,pageSize 一次取多少条
export function UtilHandleBigData(data, callback, pageSize = 100) {
  let totalCount = data.length; // 共多少条
  let currentPageNumber = 1; // 当前页数
  let totalPageNumer = Math.ceil(totalCount / pageSize); //可分多少页,就是分割为多少个小数组
  
  let handler = () => {
    let start = (currentPageNumber - 1) * pageSize;
    let end = currentPageNumber * pageSize;
    let currentData = data.slice(start, end); // 当前页的数据
    if (typeof callback === "function") {
      callback(currentData, {
        totalCount,
        totalPageNumer,
        currentPageNumber,
        pageSize,
      });
    }
    // console.log(
    //   `共${totalCount}条,共${totalPageNumer}页,当前第${currentPageNumber}条`
    // );
    if (currentPageNumber < totalPageNumer) {
      window.requestAnimationFrame(handler);
    }
    currentPageNumber++;
  };
  handler();
}

```



使用：

```js
let allData = [0, 1, 2 /**...很多条*/];
let dataView = []; // 要显示在页面的列表
UtilHandleBigData(allData, (data) => {
  dataView.push(...data);
});
```







————————————————
版权声明：本文为CSDN博主「mr.啄木鸟」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/illusion_melody/article/details/106267489