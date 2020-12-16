# Archlinux Install

```shell
$ ip link   查看网络设备
$ ip link set NETWORK_DEV up 启动网络设备
$ iwlist NETWORK_DEV scan 或者 iwlist NETWORK_DEV scan | grep ESSID  扫描wifi
$ wpa_passphrase NETWORK_DEV PASSWORD > fileName
$ wpa_supplicant -c FILE-NAME -i NETWORK_DEV &  链接网络
$ dhcpd &   动态分配网络
```
