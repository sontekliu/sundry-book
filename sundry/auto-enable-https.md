# 启用 Https

参考连接 https://diamondfsd.com/lets-encrytp-hand-https/

https://segmentfault.com/a/1190000021090685

https://sb.sb/blog/linux-acme-sh-lets-encrypt-ssl/

https://www.runoob.com/w3cnote/lets-encrypt-https.html



## 1. 安装Certbot客户端

```shell
$ curl https://get.acme.sh | sh
```

## 2. 安装证书

```shell
export Ali_Key="LTAI4GHMnPm41f861QdEYjKu"
export Ali_Secret="JQiHvWiwtCwToF8UMlEL3xF0czxBRb"
$ ~/.acme.sh/acme.sh --issue --dns dns_ali -d javaliu.com -d "*.javaliu.com"

[2020年 12月 17日 星期四 23:37:26 CST] Using CA: https://acme-v02.api.letsencrypt.org/directory
[2020年 12月 17日 星期四 23:37:26 CST] Multi domain='DNS:javaliu.com,DNS:*.javaliu.com'
[2020年 12月 17日 星期四 23:37:26 CST] Getting domain auth token for each domain
[2020年 12月 17日 星期四 23:37:37 CST] Getting webroot for domain='javaliu.com'
[2020年 12月 17日 星期四 23:37:37 CST] Getting webroot for domain='*.javaliu.com'
[2020年 12月 17日 星期四 23:37:37 CST] Adding txt value: CmX6YsMLeXmQQnNJePG_S5vI9OYPzIq_EGgSYvMOWpY for domain:  _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:37:39 CST] The txt record is added: Success.
[2020年 12月 17日 星期四 23:37:39 CST] Adding txt value: l_tW3iGXTaPo8gBbnF_0h8q8B6OY4SdKhDZgMMw9WXQ for domain:  _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:37:40 CST] The txt record is added: Success.
[2020年 12月 17日 星期四 23:37:40 CST] Let's check each DNS record now. Sleep 20 seconds first.
[2020年 12月 17日 星期四 23:38:02 CST] Checking javaliu.com for _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:38:11 CST] Domain javaliu.com '_acme-challenge.javaliu.com' success.
[2020年 12月 17日 星期四 23:38:12 CST] Checking javaliu.com for _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:38:38 CST] Please refer to https://curl.haxx.se/libcurl/c/libcurl-errors.html for error code: 52
[2020年 12月 17日 星期四 23:38:38 CST] Not valid yet, let's wait 10 seconds and check next one.
[2020年 12月 17日 星期四 23:38:50 CST] Let's wait 10 seconds and check again.
[2020年 12月 17日 星期四 23:39:01 CST] Checking javaliu.com for _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:39:01 CST] Already success, continue next one.
[2020年 12月 17日 星期四 23:39:01 CST] Checking javaliu.com for _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:39:04 CST] Domain javaliu.com '_acme-challenge.javaliu.com' success.
[2020年 12月 17日 星期四 23:39:04 CST] All success, let's return
[2020年 12月 17日 星期四 23:39:04 CST] Verifying: javaliu.com
[2020年 12月 17日 星期四 23:39:12 CST] Success
[2020年 12月 17日 星期四 23:39:12 CST] Verifying: *.javaliu.com
[2020年 12月 17日 星期四 23:39:23 CST] Success
[2020年 12月 17日 星期四 23:39:23 CST] Removing DNS records.
[2020年 12月 17日 星期四 23:39:23 CST] Removing txt: CmX6YsMLeXmQQnNJePG_S5vI9OYPzIq_EGgSYvMOWpY for domain: _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:39:26 CST] Removed: Success
[2020年 12月 17日 星期四 23:39:26 CST] Removing txt: l_tW3iGXTaPo8gBbnF_0h8q8B6OY4SdKhDZgMMw9WXQ for domain: _acme-challenge.javaliu.com
[2020年 12月 17日 星期四 23:39:28 CST] Removed: Success
[2020年 12月 17日 星期四 23:39:28 CST] Verify finished, start to sign.
[2020年 12月 17日 星期四 23:39:28 CST] Lets finalize the order.
[2020年 12月 17日 星期四 23:39:28 CST] Le_OrderFinalize='https://acme-v02.api.letsencrypt.org/acme/finalize/106218857/6793944974'
[2020年 12月 17日 星期四 23:39:30 CST] Downloading cert.
[2020年 12月 17日 星期四 23:39:30 CST] Le_LinkCert='https://acme-v02.api.letsencrypt.org/acme/cert/0467632ee82f1e74ef3d26513c04813e5835'
[2020年 12月 17日 星期四 23:39:37 CST] Cert success.
-----BEGIN CERTIFICATE-----
MIIFKzCCBBOgAwIBAgISBGdjLugvHnTvPSZRPASBPlg1MA0GCSqGSIb3DQEBCwUA
MDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQD
EwJSMzAeFw0yMDEyMTcxNDM5MjZaFw0yMTAzMTcxNDM5MjZaMBYxFDASBgNVBAMT
C2phdmFsaXUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0kBL
ezk+v1YtcjifRo1pJr78c5YYeKHhBPB5ZWP8VrKTkT9s+p9T3yMF1J49PhHf34dh
/RYtf0psOmcV+9gIFxl/N00We503tTEZnISfyfRgOYGOQ4lYkLPyV3iGv7CA9N/p
A1WiNcocwMPdqbaMqA/cwGDuUnVXWbe0YAKgYSe7sXkGnrvhEYfB1UUqiKJwSLny
lArIKgdxRv3e7eTHubRsndrBpQp9/LIzR6D2b/R79uARCmQxkGlRx0YjLbISiDSc
Hmqs1wkeS0ns+bBZ+Hzr33HEO+F/WkJdDOH63N9P1TF5vjXqCJ2tLcc9xwQhzHLA
ZgzXmqare0ttxd0pFQIDAQABo4ICVTCCAlEwDgYDVR0PAQH/BAQDAgWgMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAMBgNVHRMBAf8EAjAAMB0GA1UdDgQW
BBRzfWRdT4jv1bFzdB4XNlk+qoMGmjAfBgNVHSMEGDAWgBQULrMXt1hWy65QCUDm
H6+dixTCxjBVBggrBgEFBQcBAQRJMEcwIQYIKwYBBQUHMAGGFWh0dHA6Ly9yMy5v
LmxlbmNyLm9yZzAiBggrBgEFBQcwAoYWaHR0cDovL3IzLmkubGVuY3Iub3JnLzAl
BgNVHREEHjAcgg0qLmphdmFsaXUuY29tggtqYXZhbGl1LmNvbTBMBgNVHSAERTBD
MAgGBmeBDAECATA3BgsrBgEEAYLfEwEBATAoMCYGCCsGAQUFBwIBFhpodHRwOi8v
Y3BzLmxldHNlbmNyeXB0Lm9yZzCCAQQGCisGAQQB1nkCBAIEgfUEgfIA8AB1APZc
lC/RdzAiFFQYCDCUVo7jTRMZM7/fDC8gC8xO8WTjAAABdnFbI7YAAAQDAEYwRAIg
eBQ7w5Z9nNK8qW7AX96Sw7elBHrZ4O4w7vj5iRFPB/4CIDNPlAf/Dgqs/sFC9Nnu
hgOpbbJHPdnlSxTCt4xRWPEhAHcAb1N2rDHwMRnYmQCkURX/dxUcEdkCwQApBo2y
CJo32RMAAAF2cVsk5gAABAMASDBGAiEA+nIhfRzZAVR4NC8Gdg3ZIflBvgYMMAJx
CpvCXwMLyuECIQC5ScJ6OBTSC1Mgw06zUinQrw0Z4F+uOrJqDM99BP0/LTANBgkq
hkiG9w0BAQsFAAOCAQEAI8gt7DIt4jVSnRfKe3mCM2p20RE59yzVtxdCrupdIbMN
cV71BLSCK+IHjpqFFNxptifjzjOnO8LGvShT0Dne/cLNGO4cpb7kgi7MzrYKiHgW
OU6eYf0YIl4o3ksCvY4t4j/hsxboVXtwARV1ZXRwc7V5LqcvgtQ7SLKgG1UFp/qQ
S+qsYwh9PXIbBPRFfjZFb8U1FQSlhp8FpoCluXixaohQmj3Xw/4ueHfMbjiE83bU
IFS/5zN4VSyxnHJ3UGGdYvOEMzavR8zWh309c/nX7MiRJXVFLxaxBzsu5pMPMaOP
hgI/nQgz+DMv9ho5tHYzsWbfzkbnK1ZwrwjOSEwrQg==
-----END CERTIFICATE-----
[2020年 12月 17日 星期四 23:39:37 CST] Your cert is in  /home/sontek/.acme.sh/javaliu.com/javaliu.com.cer 
[2020年 12月 17日 星期四 23:39:37 CST] Your cert key is in  /home/sontek/.acme.sh/javaliu.com/javaliu.com.key 
[2020年 12月 17日 星期四 23:39:37 CST] The intermediate CA cert is in  /home/sontek/.acme.sh/javaliu.com/ca.cer 
[2020年 12月 17日 星期四 23:39:37 CST] And the full chain certs is there:  /home/sontek/.acme.sh/javaliu.com/fullchain.cer 
```

验证证书信息：

```shell
$ openssl x509 -in  /etc/nginx/ssl/javaliu.com.cer -noout -text 
```

不建议直接使用 acme.sh 生成的证书

```shell
$ ./.acme.sh/acme.sh -d javaliu.com -d "*.javaliu.com" --key-file ~/.acme.sh/javaliu.com/private.pem
--fullchain-file ~/.acme.sh/javaliu.com/fullchain.pem
```

