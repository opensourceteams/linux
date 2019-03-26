# nginx日志统计分析自动报表工具goaccess(推荐)
- 官网: https://goaccess.io/download

## 源码
- https://github.com/opensourceteams/linux

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


### 网站总访问量统计

- Total Requests (总请求次数)
- Valid Requests (有效的总请求次数)
- Failed Requests (无效的总请求次数)
- Not Found (不存的在页面请求次数)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/1.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/1.png)



### 按天统计访问量
- 报表展示每天访问量，包括请求量，独立的IP数，表格展示具体每天的统计量，支持分页，全量统计所有天
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (标识统计一天的日期)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/2.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/2.png)



### 按页面(不同URL)统计访问量(不包括JS、css)
- 报表展示按不同的URL统计访问量，独立IP，流量，(GET/POST)方式，页面
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Method (GET/POST)请求方式
- Data (具体的页面)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/3.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/3.png)



### 按静态页面统计访问量(包括JS、css)
- 报表展示静态资源统计访问量，独立IP，流量，(GET/POST)方式，页面
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Method (GET/POST)请求方式
- Data (静态页面URL)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/4.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/4.png)



### 不存在的页面统计访问量
- 报表展示不存在的页面统计访问量，独立IP，流量，(GET/POST)方式，页面
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Method (GET/POST)请求方式
- Data (不存在页面URL)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/5.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/5.png)



### 按不同的IP统计访问量
- 报表展示按不同的IP统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Country (国家)
- Hostname (主机名称)
- Data (IP)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/6.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/6.png)


### 按不同的操作系统统计访问量
- 按不同的操作系统统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (操作系统  Windows/Android/IOS/Linux/Unknown等)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/7.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/7.png)


### 按不同的浏览器统计访问量
- 按不同的浏览器统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (浏览器  Chrome/Safari/Firefox/MSIE等)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/8.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/8.png)



### 按时间段(小时为单位)统计访问量
- 按时间段(小时为单位)统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (小时，00/01/02/03/04/05/06/07/08/09/10/11/12/13/14/15/16/17/18/19/20/21/22/23)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/9.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/9.png)


### 按从哪里链接过来(从哪个网站跳过来)统计访问量
- 按从哪里链接过来(从哪个网站跳过来)统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (网址)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/10.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/10.png)


### 按HTTP状态码统计访问量
- 按HTTP状态码统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (HTTP状态码)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/11.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/11.png)


### 按洲统计访问量
- 按HTTP状态码统计访问量
- Hits  (请求次数/占总量的百分比)
- Visitors (当日IP个数/占总量的百分比)
- Tx.Amount (流量统计单位MB/占总量的百分比)
- Data (洲，如 亚洲/北美/南美/欧洲/大洋洲)

- https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/12.png
![](https://github.com/opensourceteams/linux/blob/master/md/images/goaccess/12.png)









