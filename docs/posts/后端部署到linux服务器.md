---
title: 后端部署到linux服务器
date: 2021-07-13 19:50:08
permalink: /pages/4a1bea/
categories:
  - 项目部署
tags:
  - linux
  - tomcat
  - redis
  - springboot
  - war
---

## 项目打包

1. 将 **springboot** 项目打包方式改为 **war**

```xml
<!-- pom.xml-->
<packaging>war</packaging>
```

2. 多模块排除内置 **tomcat**

```xml
<!-- pom.xml-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

3. 修改文件路径

```yml
# application.yml
profile: /home/projectName
```

4. 打包

项目根目录运行打包命令，在`ruoyi-admin\target`目录生成打包文件（.war结尾）

```bash
mvn clean package -Dmaven.test.skip=true
```

5. 放置打包文件

在linux服务器`/usr/local/tomcat`目录新建存放项目文件的文件夹`webapps-projectName`

## 配置tomcat

1. 打开`/usr/local/tomcat/conf/server.xml`文件
2. 新增一个`Service`，配置其端口号、证书、项目名称、文件路径等


```xml
<!-- server.xml-->
<Service name="projectName">
  <Connector port="8081"
              protocol="HTTP/1.1"
              SSLEnabled="true"
              scheme="https"
              secure="true"
              keystoreFile="/usr/local/tomcat/ssl/www.example.com.pfx"
              keystoreType="PKCS12"
              keystorePass="123456"
              clientAuth="false"
              SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
              ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256"/>
  <Engine defaultHost="localhost"
          name="projectName">
    <Realm className="org.apache.catalina.realm.LockOutRealm">
      <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
              resourceName="UserDatabase"/>
    </Realm>
    <Host name="localhost"
          appBase="webapps-projectName"
          unpackWARs="true"
          autoDeploy="true">
      <Context path="/"
                docBase="projectName"
                reloadable="false"
                crossContext="true"/>
      <Valve className="org.apache.catalina.valves.AccessLogValve"
              directory="logs"
              prefix="localhost_access_log"
              suffix=".txt"
              pattern="%h %l %u %t &quot;%r&quot; %s %b"/>
      <Valve className="org.apache.catalina.valves.RemoteIpValve"
              remoteIpHeader="X-Forwarded-For"
              protocolHeader="X-Forwarded-Proto"
              protocolHeaderHttpsValue="https"/>
    </Host>
  </Engine>
</Service>
```
- `projectName`替换成自己的项目名称
- `port`端口号根据实际情况设置
- `keystoreFile`放置ssl证书，[阿里云ssl证书申请](https://blog.csdn.net/yunweifun/article/details/123409692)

## 启动服务

1. 启动redis服务

在`/usr/local/redis/bin`目录中执行`nohup redis-server &`命令启动`redis`服务

2. 配置mysql数据库

- 将项目所用数据库的结构和数据导出为`sql`文件
- 将`sql`文件放在`/sql`目录中
- 在linux服务器执行命令`mysql -u root -p`登录数据库
- 新建数据库`create database dn_name;`
- 切换至新建的数据库`use dn_name;`
- 导入数据库`source /sql/fileName.sql;`

3. 启动tomcat服务

- tomcat还未运行的情况下，在`/usr/local/tomcat/bin`目录运行`nohup ./startup.sh &`命令
- 否则先运行`./shutdown.sh`命令关闭tomcat服务，再运行`nohup ./startup.sh &`启动服务
