# 启用 Https

参考连接 https://diamondfsd.com/lets-encrytp-hand-https/

https://segmentfault.com/a/1190000021090685

https://sb.sb/blog/linux-acme-sh-lets-encrypt-ssl/

https://www.runoob.com/w3cnote/lets-encrypt-https.html



## 1. 安装Certbot客户端

```shell
$ curl https://get.acme.sh | sh
`i``

## 2. 安装证书

```shell
export Ali_Key="LTAI4GHMnPm41f861QdEYjKu"
export Ali_Secret="JQiHvWiwtCwToF8UMlEL3xF0czxBRb"
$ ~/.acme.sh/acme.sh --issue --standalone -d javaliu.com -d *.javaliu.com
```

