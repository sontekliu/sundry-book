## Maven私服安装部署

#### 1. 首先确定我们的系统已安装好maven和JDK并配置好环境变量

#### 2. 官网下载nexus，下载地址是：http://www.sonatype.org/nexus/go   
	我安装的版本是nexus-2.14.1-01
#### 3. 解压安装包
	```
		$ makdir nexus
		$ tar -zxvf nexus-2.14.1-01-bundle.tar.gz
		$ ls 
		nexus-2.14.1-01  sonatype-work
	```
#### 4. 启动nexus  
	```
		$ cd nexus-2.14.1-01
		$ ./nexus start
		$ ./nexus
		Usage: ./nexus {console | start | stop | status | restart | dump}
	```
#### 5. 打开浏览器访问：http://127.0.0.1:8081/nexus

#### 6. 登录系统，用户名和密码分别是 admin/admin123

#### 7. POM.xml配置

