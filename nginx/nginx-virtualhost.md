# nginx 虚拟主机的配置

在 `nginx.conf` 文件中，`worker_processes 1` 表示工作进程的个数，
一般设置CPU个数 * 核数。

```
events {
    // 表示每个工作进程的链接数
    worker_connections  1024
}
```

## 虚拟主机配置

准备工作:
在 `/usr/local/nginx/html` 目录下创建 `prot`、`host`、`ip` 等目录。并且在相应的目录下创建 `index.html` 文件，分别输入相应的内容。
在 `/etc/hosts` 添加 `192.168.1.100 z.com`  

#### 基于端口的虚拟主机
```
    server {
        listen  8080;
        server_name z.com;
        location / {
            root html/port;
            index index.html;
        }
    }
```

#### 基于域名的虚拟主机
```
    server {
        listen  80;
        server_name z.com;
        location / {
            root html/host;
            index index.html;
        }
    }
```

#### 基于 IP 的虚拟主机
```
    server {
        listen  80;
        server_name 192.168.1.104;
        location / {
            root html/host;
            index index.html;
        }
    }
```







