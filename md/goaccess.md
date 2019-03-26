# goaccess  分析nginx日志使用说明
- 官网: https://goaccess.io/download

## 图表
- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess.html
- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess.png)

## 安装(centos)
```aidl
 wget https://tar.goaccess.io/goaccess-1.3.tar.gz
$ tar -xzvf goaccess-1.3.tar.gz
$ cd goaccess-1.3/
$ ./configure --enable-utf8 --enable-geoip=legacy
$ make
# make install
```

## 安装(mac)
```aidl
 brew install goaccess
```


### 修改配置
- /usr/local/etc/goaccess/goaccess.conf
- date-format 以nginx的access.log实际日志记录格式为准

```aidl

time-format %H:%M:%S
date-format %d/%b/%Y
log-format %h %^[%d:%t %^] "%r" %s %b "%R" "%u"
```


### 控制台分析
```aidl
 goaccess -a -d -f /usr/local/nginx/logs/access.log -p /usr/local/etc/goaccess/goaccess.conf 
```


### HTML台分析(推荐)
- HTML分析的数据很完善，还有报表，种类其全
- html/goaccess.html 为生成html文件路径
- 

```aidl
goaccess -a -d -f /usr/local/nginx/logs/access.log -p /usr/local/etc/goaccess/goaccess.conf  -o html/goaccess.html
```