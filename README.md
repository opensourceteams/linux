# linux命令

### nginx 日志统计分析常用命令
- https://github.com/opensourceteams/linux/blob/master/md/nginx-log.md



## nc命令

### nc发送消息
- 发送消息
- 发送端和接收端都起来后，就可以在任意一端的终端输入数据，另一端就会同步消息

```aidl
 nc -l 1234

```



### nc接收消息
- 拉收消息
- 发送端和接收端都起来后，就可以在任意一端的终端输入数据，另一端就会同步消息

```aidl
 nc 127.0.0.1 1234

```


### ls 过滤文件夹并删除
- ls过滤当前路径下面的所有文件夹并删除(强制)

```aidl

 ls -l |grep ^d  | xargs rm -rf
```
