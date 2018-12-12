### MongoDB 3.2.10 安装
```
	参考官方文档:http://dev.mysql.com/doc/refman/5.5/en/binary-installation.html
```
MongoDB 的安装方式有多种：   
1、基于源代码的安装   
2、基于YUM源的方式安装
3、基于tar包的方式，即，解压配置即可

```
1、首先官方网站下载MongoDB 企业tar包
2、创建mongodb组以及mongodb用户
	groupadd mongodb
	useradd -r -g mongodb -s /bin/nologin mongodb
3、将下载的二进制文件解压到 /usr/local 下面
	mv mongodb-xxx.tgz /usr/local/
	tar -zxvf mongodb-xxx.tgz
4、创建mysql软连接到解压后的目录
	ln -s mongodb-xxx mongo
5、修改mongodb属主和属组
	cd	mongo
	chown -R mongodb.mongodb .
6、创建数据存储目录和日志目录
	mkdir -p /usr/local/mongo/data/db
	chown -R mongodb.mongodb ./data/db
	
	mkdir -p /usr/local/mongo/data/log
	chown -R mongodb.mongodb ./data/log
7、创建配置文件
	mongo]# touch mongodb.conf
	mongo]# vim mongodb.conf

	dbpath=/usr/local/mongo/data/db
	port=27017
	logpath=/usr/local/mongo/data/log/mongodb.log
	logappend=true
	auth=true	# 添加用户认证

8、 启动mongodb
	mongo]# mongod -f /usr/local/mongo/mongodb.conf &
```



