# Linux 杀掉指定端口号的java进程
- 杀掉端口号为 9999 的java程序
```aidl
kill -9 ` netstat -nlp | grep :9999 | awk '{print $7}' | awk -F"/" '{ print $1 }' `
```