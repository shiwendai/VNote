# linux命令
* 查看端口占用情况：netstat -tunlp |grep 8000
```
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

* 查找文件命令
```
find /home/ -name '*elm*'
```