# SpringBoot 有 jar 改成 war 部署

本文是基于 SpringBoot 2.0 以上版本。注意，SpringBoot 2.0 需要 Tomcat 8.5 以上版本。

默认情况下，SpringBoot 是打成 jar 包的，但是有人可能更熟悉打成 war 包放到 Tomcat 里面部署。

SpringBoot 有 jar 改成 war 大约有如下步骤：

> 1. 修改 pom.xml 文件，将 jar 改成 war
> 2. 使内置的 Servlet 容器为 provided
> 3. 启动类继承 SpringBootServletInitializer
> 4. 部署

#### 1. 修改 pom.xml 文件，将 jar 改成 war

```xml
<packaging>war</packaging>
```

#### 2. 修改 pom.xml 使内置的 Servlet 容器为 provided

修改 pom.xml 添加如下内容：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency
```

#### 3. 启动类继承 SpringBootServletInitializer

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.support.SpringBootServletInitializer;

@SpringBootApplication
public class SpringBootWebApplication extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application){         return application.sources(SpringBootWebApplication.class);
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(SpringBootWebApplication.class, args);
    }
}
```

#### 4. 部署

部署分为本地部署和远程部署，本地部署即通过本地的 IDE 进行启动调试；远程部署是指部署到某台远程集群的 Tomcat 

1. 本地部署，此处使用 IDEA 为例。

   ```shell
   mvn spring-boot:run
   ```

   注意：此处直接运行 main 方法好像不行。

2. 打包远程部署

   ```shell
   mvn clean package
   ```

   将打出的 war 文件，即 /target/xxxx-version.war 文件上传到指定的 tomcat 启动即可。



**参考资料**

https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#howto-create-a-deployable-war-file

https://www.mkyong.com/spring-boot/spring-boot-deploy-war-file-to-tomcat/











































