# nginx 日志分析
- https://www.cnblogs.com/gouge/p/7089939.html



## 常用

### 统计某天的IP个数
```aidl
 grep "26/Mar/2019" /usr/local/nginx/logs/access.log | awk '{print $1}'  | sort -n | uniq | wc -l
```


### 统计某天的PV(总访问次数)
```aidl
 grep "26/Mar/2019" /usr/local/nginx/logs/access.log | wc -l
```


### 统计一天IP访问次数
```aidl
  grep "26/Mar/2019" /usr/local/nginx/logs/access.log | awk '{print $1}' | sort -n |uniq -c | sort -rn | head -n 100
```

### 统计某天的IP访问次数
```aidl
  grep "26/Mar/2019" /usr/local/nginx/logs/access.log | awk '{print $1}' | sort -n |uniq -c | sort -rn | head -n 100
```


----------------------


## IP相关统计
- 统计IP访问量（独立ip访问数量）

```aidl
awk '{print $1}' access.log | sort -n | uniq | wc -l

```

- 查看某一时间段的IP访问量(4-5点)

```aidl
grep "07/Apr/2017:0[4-5]" access.log | awk '{print $1}' | sort | uniq -c| sort -nr | wc -l  
```

- 查看访问最频繁的前100个IP

```aidl
awk '{print $1}' access.log | sort -n |uniq -c | sort -rn | head -n 100
```

- 查看访问100次以上的IP

```aidl
awk '{print $1}' access.log | sort -n |uniq -c |awk '{if($1 >100) print $0}'|sort -rn
```

- 查询某个IP的详细访问情况,按访问频率排序

```aidl
grep '127.0.01' access.log |awk '{print $7}'|sort |uniq -c |sort -rn |head -n 100
```


### 页面访问统计
- 查看访问最频的页面(TOP100)


```aidl
awk '{print $7}' access.log | sort |uniq -c | sort -rn | head -n 100
```

- 查看访问最频的页面([排除php页面】(TOP100)

```aidl
grep -v ".php"  access.log | awk '{print $7}' | sort |uniq -c | sort -rn | head -n 100 
```

- 查看页面访问次数超过100次的页面

```aidl
cat access.log | cut -d ' ' -f 7 | sort |uniq -c | awk '{if ($1 > 100) print $0}' | less
```

- 查看最近1000条记录，访问量最高的页面

```aidl
tail -1000 access.log |awk '{print $7}'|sort|uniq -c|sort -nr|less
```

- 每秒请求量统计
- 统计每秒的请求数,top100的时间点(精确到秒)

```aidl
awk '{print $4}' access.log |cut -c 14-21|sort|uniq -c|sort -nr|head -n 100
```

### 每分钟请求量统计
- 统计每分钟的请求数,top100的时间点(精确到分钟)

```aidl
awk '{print $4}' access.log |cut -c 14-18|sort|uniq -c|sort -nr|head -n 100
```

- 每小时请求量统计
- 统计每小时的请求数,top100的时间点(精确到小时)

```aidl
awk '{print $4}' access.log |cut -c 14-15|sort|uniq -c|sort -nr|head -n 100
```

性能分析
在nginx log中最后一个字段加入$request_time

- 列出传输时间超过 3 秒的页面，显示前20条

```aidl
cat access.log|awk '($NF > 3){print $7}'|sort -n|uniq -c|sort -nr|head -20
```

- 列出php页面请求时间超过3秒的页面，并统计其出现的次数，显示前100条
```aidl
cat access.log|awk '($NF > 1 &&  $7~/\.php/){print $7}'|sort -n|uniq -c|sort -nr|head -100
```

蜘蛛抓取统计
- 统计蜘蛛抓取次数
```aidl
grep 'Baiduspider' access.log |wc -l
```

- 统计蜘蛛抓取404的次数
```aidl
grep 'Baiduspider' access.log |grep '404' | wc -l
```

TCP连接统计
- 查看当前TCP连接数
```aidl
netstat -tan | grep "ESTABLISHED" | grep ":80" | wc -l
```

- 用tcpdump嗅探80端口的访问看看谁最高

```aidl

tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F"." '{print $1"."$2"."$3"."$4}' | sort | uniq -c | sort -nr

```

