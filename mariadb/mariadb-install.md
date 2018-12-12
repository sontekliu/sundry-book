##MariaDB安装过程   

关于MariaDB的优点在这我就不多说了，大家可以自行百度一下。下面我主要讲解一下MariaDB安装过程以及注意事项。

1、首先修改YUM源(YUM源地址：http://yum.mariadb.org/) 在/etc/yum.respo.d/目录下添加MariaDB.repo，内容如下：
```
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/centos6-amd64/
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB 
gpgcheck=1
```

2、使用YUM命令安装，命令如下：       
```
yum -y install MariaDB-client MariaDB-server
```

3、安装无误后启动MariaDB服务       
```
service mysql status #查看状态
service mysql start #启动数据库
service mysql stop #停止数据库
```

4、修改root密码 
```
mysqladmin -u root password 'lsjlsj'
```

5、登录数据库     
```
mysql -h 192.168.1.100 -P 3306 -u root -p
```

6、赋予远程访问的权限
```
首先登录mysql     
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option   
```

7、注意事项：       
* 关闭selinux    
* mysql的默认安装路径为：/usr/share/mysql 数据库的默认位置为：/var/lib/mysql    
* 此时 /etc/my.cnf 仅仅包含一句 指向 /etc/my.cnf.d目录，此时修改MySQL的配置可以修改/etc/my.cnf.d/server.cnf，但是不能删除 my.cnf文件





