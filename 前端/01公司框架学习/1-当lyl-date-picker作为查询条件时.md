# 1-当lyl-date-picker作为查询条件时

由于时间选择器的的限制条件，当时间选择器作为上面的查询时，有几个注意点：

1. 需要设置为：`type="datetimerange"`，应为如果用其他的，时，分，秒部分默认都是空的，或者为0，这样后端在查询数据库的时候，是查不出来的。
2. 需要将时、分、秒设置在一个正常的区间内，设置方法`:default-time="['00:00:00', '23:59:59']"`，因为，`datetimerange`默认的时分秒为`00:00:00 ~ 00:00:00`数据库查询的时候还是会有问题，所以我们手动设置一下默认值，让时间处于一个正常的范围内。



所以正常代码：

```vue
<lyl-search-col :lg="8" :md="12" :sm="12" :xl="12">
  <lyl-date-picker :default-time="['00:00:00', '23:59:59']" v-model="dataForm.registerTime" type="datetimerange" custom></lyl-date-picker>
  
  <!--注意，这里具体是使用v-model，还是:start.sync="dataForm.startTm" :end.sync="dataForm.endTm"看接口参数情况，给1个使用v-model，给2个使用后面的-->
</lyl-search-col>
```



>注意，如果用了grid的mixins，要求要GE，startTm和LE，endTm需要在dataMode里设置一下
>
>```js
>dataMode: {
>  crtTm: 'BT',
>  startTm: 'GE',
>  endTm: 'LE'
>}
>```

