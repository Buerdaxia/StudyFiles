# windows常用命令



## 指定端口查询进程

```
netstat -ano|findstr "8080"

// 查询8080端口下的进行
```



## 利用进程PID结束进程

```
taskkill /pid 1234 /f

// 结束pid为1234的进程，/f标识强制
```





## ping测试网络连通性

```
ping xxx
// 测试网络连通性
```

