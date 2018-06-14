---
title: mycat使用
date: 2018-05-10 12:11:12
categories:
- 工具
tags:
- 工具
---


## mycat使用

#### 官网

- [http://www.mycat.io](http://www.mycat.io)
- [http://www.mycat.io/document/Mycat_V1.6.0.pdf](http://www.mycat.io/document/Mycat_V1.6.0.pdf)
- [https://github.com/MyCATApache/Mycat-Server](https://github.com/MyCATApache/Mycat-Server)


#### mycat-server安装配置
- 打开地址[http://dl.mycat.io](http://dl.mycat.io)，下载对应版本的mycat-server安装文件，解压。修改配置server.xml，
```xml
<!-- TESTDB为mycat对外提供的db名称，此db名称需与schema.xml中<schema name="TESTDB">对应 -->
<user name="root">
                <property name="password">123456</property>
                <property name="schemas">TESTDB</property>
</user>
```
schema.xml配置
```xml
<mycat:schema xmlns:mycat="http://io.mycat/">

        <schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100" dataNode="dn1">
        </schema>
        <!-- database为实体数据库中的数据库实例名 -->
        <dataNode name="dn1" dataHost="localhost1" database="testdb" />

        <dataHost name="localhost1" maxCon="1000" minCon="10" balance="0"
                          writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
                <heartbeat>select user()</heartbeat>

<!-- writeHost和readHost对应实体数据库连接的配置，以下配置是一个简单的读写分离, 数据库的主从配置需数据库自行配置 -->                
                <writeHost host="hostM1" url="localhost:3306" user="root"
                                   password="123456">
                        <readHost host="hostS2" url="localhost:13306" user="root" password="123456" />
                </writeHost>
        </dataHost>
</mycat:schema>
```
配置完成后，执行`mycat start`,即可启动mycat服务。使用数据库工具连接8066端口，即可像使用普通mysql数据库一样使用mycat。


#### mycat-eye安装配置

- 打开地址[http://dl.mycat.io](http://dl.mycat.io)，下载对应版本的mycat-web安装文件，解压。修改配置mycat-web/mycat-web/WEB-INF/classes/mycat.properties，修改其中的zookeeper为自己的zookeeper服务地址。执行start.sh即可启动mycat-eye，登陆$SERVER_IP:8082/mycat，即可打开mycat-eye管理页面。



