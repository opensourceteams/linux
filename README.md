# linux命令

### nginx日志统计分析自动报表工具goaccess(推荐)
- https://github.com/opensourceteams/linux/blob/master/md/goaccess.md


### nginx 日志统计分析常用命令
- https://github.com/opensourceteams/linux/blob/master/md/nginx-log.md

### linux压缩和解压缩命令
- https://github.com/opensourceteams/linux/blob/master/md/linux-compression.md


## iftop 命令(流量监控)
- https://github.com/opensourceteams/linux/blob/master/md/linux-iftop.md


## linux 网速测试(上传/下载速度)
```aidl
wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod +rx speedtesti.py
./speedtesti.py

```
- https://github.com/opensourceteams/linux/blob/master/md/images/speedtesti/speedtesti.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/speedtesti/speedtesti.png)

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




### 显示20分钟前的文件

```aidl
find /home/prestat/bills/test -type f -mmin +20 -exec ls -l {} \;
```


### 删除20分钟前的文件

```aidl
find /home/prestat/bills/test -type f -mmin +20 -exec rm {} \;
```


### 显示20天前的文件

```aidl
find /home/prestat/bills/test -type f -mtime +20 -exec ls -l {} \;
```


### 删除20天前的文件

```aidl
find /home/prestat/bills/test -type f -mtime +20 -exec rm {} \;

```


下面为find命令的参数说明：

-name 按照文件名查找文件。
-perm 按照文件权限来查找文件。
-prune 使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。
-user 按照文件属主来查找文件。
-group 按照文件所属的组来查找文件。
-mtime -n +n 按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内，+ n表示文件更改时间距现在n天以前。

find命令还有-atime和-ctime 选项，但它们都和-m time选项。
-nogroup 查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。
-nouser 查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。
-newer file1 ! file2 查找更改时间比文件file1新但比文件file2旧的文件。
-type 查找某一类型的文件，诸如：b - 块设备文件，d - 目录，c - 字符设备文件，p - 管道文件，l - 符号链接文件，f - 普通文件。
-size n：[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。
-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。
-fstype：查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。
-mount：在查找文件时不跨越文件系统mount点。
-follow：如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。
-cpio：对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。

另外,下面三个的区别:
-amin n　　查找系统中最后N分钟访问的文件
-atime n　　查找系统中最后n*24小时访问的文件
-cmin n　　查找系统中最后N分钟被改变文件状态的文件
-ctime n　　查找系统中最后n*24小时被改变文件状态的文件
-mmin n　　查找系统中最后N分钟被改变文件数据的文件
-mtime n　　查找系统中最后n*24小时被改变文件数据的文件