# 查看远程服务器开放的端口号

### mac 安装nmap命令
```aidl
brew install nmap
```

### 查看远程服务端开放了哪些端口
```aidl
nmap b0.com
Starting Nmap 7.70 ( https://nmap.org ) at 2019-05-01 19:09 CST
Nmap scan report for b0.com (106.13.139.60)
Host is up (0.11s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
3306/tcp open  mysql
8080/tcp open  http-proxy

```