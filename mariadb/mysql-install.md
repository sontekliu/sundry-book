# MySQL 安装

MySQL 的安装方式有多种：   
1、基于源代码的安装   
2、基于rpm包的安装  
3、基于MySQL二进制发行版的安装      
本文主要讲解的是基于MySQL二进制发行版的安装，MySQL二进制发行版主要是指官方已经
编译好，并压缩好的.tar.gz文件，直接解压缩进行相关的配置即可。   

### MySQL5.5.52 安装

```
参考官方文档:http://dev.mysql.com/doc/refman/5.5/en/binary-installation.html

1、首先官方网站下载MySQL二进制发行版包
2、创建mysql组以及mysql用户
	groupadd mysql
	useradd -r -g mysql -s /bin/nologin mysql
3、将下载的二进制文件解压到 /usr/local 下面
	mv mysql-VERSION-OS.tar.gz /usr/local/
	tar -zxvf mysql-VERSION-OS.tar.gz
4、创建mysql软连接到解压后的目录
	ln -s mysql-VERSION-OS mysql
5、修改mysql属主和属组
	cd mysql
	chown -R mysql.mysql .
6、创建数据存储目录
	mkdir -p /opt/mydata/data	
	chown -R mysql.mysql /opt/mydata/data
7、执行数据脚本
	script/mysql_install_db --user=mysql --datadir=/opt/mydata/data
8、还原mysql安装目录下面的属主为root
	chown -R root .
9、将执行文件复制到init.d目录下
	cp support-files/mysql.server /etc/init.d/mysqld
10、复制配置文件到/etc/my.cnf
	cp support-files/my-medium.cnf /etc/my.cnf
11、修改配置文件添加如下内容
	datadir = /opt/mydata/data
12、启动mysql
	service mysqld start
13、添加path变量
	在/etc/profile.d/目录下创建mysql.sh
	并添加：
		export PATH=$PATH:/usr/local/mysql/bin
14、重新登录系统之后，运行mysql登录mysql数据库
15、清除匿名用户
	drop user ''@'localhost';
	drop user ''@'localhost.localdomain'
16、修改root密码   
	mysql> UPDATE user SET password = PASSWORD('lsjlsj') WHERE user = 'root';
	mysql> FLUSH PRIVILEGES;
```

### MySQL5.7.17 安装

```
参考官方文档:https://dev.mysql.com/doc/refman/5.7/en/binary-installation.html  

1、首先官方网站下载MySQL二进制发行版包
2、创建mysql组以及mysql用户
	groupadd mysql
	useradd -r -g mysql -s /bin/false mysql
3、将下载的二进制文件解压到 /usr/local 下面
	mv mysql-VERSION-OS.tar.gz /usr/local/
	tar -zxvf mysql-VERSION-OS.tar.gz
4、创建mysql软连接到解压后的目录
	ln -s mysql-VERSION-OS mysql
5、修改权限
    $> cd mysql
    $> mkdir mysql-files
    $> chmod 750 mysql-files
6、修改mysql属主和属组
	cd mysql
	chown -R mysql.mysql .
7、创建数据存储目录
	mkdir -p /opt/mydata/data	
	chown -R mysql.mysql /opt/mydata/data
8、执行数据脚本
    $> bin/mysqld --initialize --user=mysql --datadir=/opt/mydata/data
    注意：记录下创建数据库的密码
    $> bin/mysql_ssl_rsa_setup
    $> chown -R root .
    $> chown -R mysql mysql-files
9、将执行文件复制到init.d目录下，使其开机启动
    $> cp support-files/mysql.server /etc/init.d/mysqld
10、复制配置文件到/etc/my.cnf
	cp support-files/my-medium.cnf /etc/my.cnf
11、修改配置文件添加如下内容
	datadir = /opt/mydata/data
12、启动mysql
    service mysqld start
13、添加path变量
	在/etc/profile.d/目录下创建mysql.sh
	并添加：
	export PATH=$PATH:/usr/local/mysql/bin
14、重新登录系统之后，运行mysql登录mysql数据库
    $> mysql -uroot -p
    Enter password: 输入之前记录下的密码
15、重置密码  
    mysql> set password = PASSWORD('lsjlsj');
```



