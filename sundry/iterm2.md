# iterm2 配置

### 1. ssh 别名配置

使用 Mac 每次登录某台远程机器的时候都是输入如下内容 ssh xxx@192.168.1.100。
这种方式缺点：
    1. 输入内容较多
    2. IP 地址也记不住
    3. 影响效率，操作较慢

如果能使用别名就好了，类似这种 `ssh aliasname`，最近网上找到了这种方式。

在 `~/.ssh/` 目录下创建 `config` 文件。配置如下内容：

```
Host            Aliyun              # 别名
HostName        192.168.1.100       # 主机 IP 地址
Port            22                  # 连接主机的端口号
User            root                # 登录主机时的用户
IdentityFile    ~/.ssh/id_rsa       # 使用私钥的文件地址

#  如果配置多个则直接复制多份接口

Host            Aliyun2             # 别名
HostName        192.168.2.100       # 主机 IP 地址
Port            22                  # 连接主机的端口号
User            root                # 登录主机时的用户
IdentityFile    ~/.ssh/id_rsa       # 使用私钥的文件地址
```




