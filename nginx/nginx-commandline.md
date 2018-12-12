# Nginx 信号 量

```
    -v 输出当前Nginx版本
    -V 输出Nginx版本，编译版本，配置文件参数
    -t 不运行，仅仅测试配置文件的正确性
    -s signal 向主线程发送信号，stop, quit, reopen, reload
    -c filename 让Nginx使用指定的配置文件，而不是默认的配置文件
```

Nginx 启动之后，可以有两种方式控制Nginx程序：

> 第一，使用 `nginx` 命令，使用 `-s` 参数，例如：`/usr/local/nginx/sbin/nginx -s stop`   
> 第二，向 Nginx主进程 发送信号，默认主进程号的位置是：`/usr/local/nginx/logs/nginx.pid`   

```
    TERM, INT   快速杀掉进程
    QUIT        优雅的杀掉进程，即请求结束后再关闭，常用
    HUP         重新加载配置文件，使用新的配置文件开启一个新的进程，优雅的杀掉老进程
    USER1       重新打开日志文件，日志分割时有用
    USER2       平滑升级
    WINCH       优雅的关闭旧进程，配合 USER2 进行升级
```

例如：   
`kill -QUIT $( cat /usr/local/nginx/logs/nginx.pid )`







