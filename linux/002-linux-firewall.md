
# Linux 防火墙

### 1. firewall 基本使用

1. 开机启用 systemctl enable firewalld.service
2. 开机禁用 systemctl disable firewalld.service
3. 启动防火墙 systemctl start firewalld.service
4. 重启防火墙 systemctl restart firewalld.service
5. 关闭防火墙 systemctl stop firewalld.service
6. 查看状态 systemctl status firewalld.service
7. 查看是否开机启动 systemctl is-enabled firewalld.service
8. 查看已启动的服务列表 systemctl list-unit-files | grep enabled
9. 查看启动失败的服务列表 systemctl --failed

### 2. 配置 firewalld-cmd 
1. 查看帮助  firewall-cmd --help
2. 查看状态  firewall-cmd --state
3. 查看所有打开的端口 firewall-cmd --zone=public --list-ports
4. 更新防火墙规则 firewall-cmd --reload
5. 查看区域信息  firewall-cmd --get-active-zones
6. 查看指定接口所属区域 firewall-cmd --get-zone-of-interface=eth0
7. 拒绝所有包 firewall-cmd --panic-on
8. 取消拒绝状态 firewall-cmd --panic-off
9. 查看是否拒绝 firewall-cmd --query-panic
10. 开启端口 firewall-cmd --zone=public --add-port=80/tcp --permanent (永久生效)
11. 查看端口 firewall-cmd --zone=public --query-port=80/tcp
12. 删除端口 firewall-cmd --zone=public --remove-port=80/tcp --permanent





