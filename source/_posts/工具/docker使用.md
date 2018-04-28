---
title: docker使用
date: 2016-12-10 18:08:45
categories:
- 工具
tags:
- 工具
- 数据库
---

# docker使用

## docker安装


#### 安装

- [docker官方安装](https://docs.docker.com/install/)
- [国内源安装docker](https://yq.aliyun.com/articles/110806)


#### docker安装
1. 登录docker官网，下载docker安装包，安装
2. Preferences -> File Sharing添加需共享的文件夹

#### 参考资料
[https://www.docker.com](https://www.docker.com)

## docker启动

- `sudo systemctl start docker`

## docker命令

#### 启动容器
- `docker start <容器名／容器ID>`

#### 重启容器
- `docker restart <容器名／容器ID>`

#### 停止容器
- `docker stop <容器名／容器ID>`

#### 删除容器
- `docker rm <容器名／容器ID>`

#### 删除镜像
- `docker rmi <镜像名>`

#### 连接容器
- `docker exec -it <容器名／容器ID> /bin/bash`


#### 复制文件
- `docker cp <容器名／容器ID>:容器文件路径 主机路径`


#### 创建容器
- `docker run`
> -d 使用后台模式运行
> --name 为容器命名
> -t 通过shell进行交互，-i 使用管道命令，通常-it连用
> -p 映射端口，主机端口:容器端口
> -v 映射目录，主机目录:容器目录，将主机目录挂载到容器目录上


#### docker安装程序

```
# 先更新源
apt-get update;
apt install vim;
```


## docker-compose命令

#### 创建或再建服务
- `docker-compose build`

#### 帮助
- `docker-compose help`

#### 显示服务的日志输出
- `docker-compose logs <容器名／容器ID> ` 

#### 显示容器
- `docker-compose ps`

#### 设置为一个服务启动的容器数量
- `docker-compose scale <服务名=数量>` 

#### 为一个服务构建、创建、启动、附加到容器 
- `docker-compose up`
- `docker-compose up -d` 后台启动



## Dockerfile编写

#### Dockerfile命令
- 注释 #
- FROM 基础镜像
- RUN 运行命令
- ADD 添加文件到镜像
- WORKDIR 切换工作目录
- EXPOSE 定义导出端口
- CMD 镜像完成执行命令

## docker构建镜像

#### 基于java构建运行springboot程序的镜像
1. 编写Dockerfile
```dockerfile
FROM java:7
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ADD app.tar /root/
WORKDIR /root/app/
EXPOSE 8080
CMD ["java","-jar","app.jar"]
```

2. 运行构建命令
`docker build -t xy/app .`

3. 运行镜像
`docker run -d --name app -p 8080:8080 xy/app`

#### docker管理工具

###### Kitematic
- 功能：Docker GUI工具，可以方便通过GUI管理docker容器。

###### Swarm
- 功能：管理docker集群，将多台机器的docker服务统一进行管理。

#### docker监控工具
- CoScale 提供对容器的监控

#### 常用工具镜像

- Adminer 流行的开源数据库管理工具，支持包括MySQL, PostgreSQL, SQLite, MS SQL, Oracle, SimpleDB, Elasticsearch, MongoDB多种数据库，使用PHP开发。

# docker安装常用服务

## docker安装mysql数据库

#### mysql安装
```bash
docker search mysql;
docker pull mysql;
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /opt/docker/mysql:/var/lib/mysql  mysql;
```

#### 登录数据库
```
hostname: localhost
port: 3306
username: root
password: 123456
```

#### 参考资料
[https://hub.docker.com/_/mysql/](https://hub.docker.com/_/mysql/)

## docker安装oracle数据库

#### oralce安装
```bash
docker search oracle;
docker pull wnameless/oracle-xe-11g;
docker run -d --name oracle -p 11122:22 -p 11521:1521  -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g;
```

#### 远程连接
```bash
ssh root@localhost -p 11122;
password: admin;
```

#### 登录数据库
```
hostname: localhost
port: 11521
sid: XE
username: system
password: oracle
```

#### 参考资料
[https://hub.docker.com/r/wnameless/oracle-xe-11g/](https://hub.docker.com/r/wnameless/oracle-xe-11g/)

#### 坑
- sid是xe而不是orcl
- 登录后为root用户目录，虽然能使用sqlplus命令，但无法成功连接到oracle，需要切换到oracle用户下连接，这与普通linux下的oralce数据库是一致的。

## docker安装redis数据库

#### 安装redis
```bash
docker search redis;
docker pull redis;
docker run --name redis -d  -v  /opt/docker/redis/data:/data -p 6379:6379  redis redis-server --appendonly yes;
```

#### 登录数据库
```
hostname: localhost
port: 6379
username: 
password: 
```

#### 参考资料
[https://hub.docker.com/_/redis/](https://hub.docker.com/_/redis/)

## docker安装rabbotmq消息队列

#### 安装rabbitmq
```bash
docker search rabbitmq:3-management;
docker pull rabbitmq:3-management;
docker run -d --name rabbitmq --hostname rabbitmq  -e RABBITMQ_DEFAULT_USER=user -e  RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 -p 5672:5672 -v /opt/docker/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management;
```

#### 连接rabbitmq
```
hostname: localhost
port: 5672
username: user
password: 123456
```

#### 登录rabbitmq管理系统
[http://localhost:15672](http://localhost:15672)

#### 参考资料
[https://hub.docker.com/_/rabbitmq/](https://hub.docker.com/_/rabbitmq/)

## docker安装elk日志分析平台

#### 安装elk
```
docker search elk;
docker pull sebp/elk;
docker run -d -it --name elk -p 5601:5601 -p 9200:9200 -p 5044:5044 sebp/elk;
```

#### 登录管理平台
- http://localhost:5601

#### 配置log4j2连接elk平台
- 为了便于测试，去除默认的ssl安全配置

##### 配置elk
- 由于log4j2需使用tcp插件发送日志数据，所以修改容器/etc/logstash/conf.d/02-beats-input.conf内容为
```
input {
  tcp {
    port => 5044
  }
}
```

##### 配置log4j2
- 配置log4j2.xml文件,实现通过tcp发送日志给logstash
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="warn">
  <properties>
        <property name="LOG_PATTERN">%d{HH:mm:ss} %-5level %msg%n</property>
        <property name="LOG_ENCODING">utf-8</property>
    </properties>
  <Appenders>
    <Socket name="socket" host="localhost" port="5044">
     <PatternLayout chatset="${LOG_ENCODING}" pattern="${LOG_PATTERN}"/>
      <SerializedLayout />
    </Socket>
  </Appenders>
  <Loggers>
    <Root level="info">
      <AppenderRef ref="socket"/>
    </Root>
  </Loggers>
</Configuration>
```

##### 查看日志信息
- 登录kibana管理平台，即可查看到接收的日志信息

#### 配置filebeat连接elk平台

##### 配置filebeat
- filebeat是一个开源的日志收集器，常与elk配合使用

#### 参考资料
[http://elk-docker.readthedocs.io](http://elk-docker.readthedocs.io)
[https://www.elastic.co/guide/en/logstash/current/introduction.html](https://www.elastic.co/guide/en/logstash/current/introduction.html)
[https://www.elastic.co/guide/en/logstash/current/input-plugins.html](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)
[https://www.elastic.co/guide/en/logstash/current/plugins-codecs-plain.html](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-plain.html)
[http://logging.apache.org/log4j/2.x/manual/appenders.html#SocketAppender](http://logging.apache.org/log4j/2.x/manual/appenders.html#SocketAppender)
[http://kibana.logstash.es/content/kibana/index.html](http://kibana.logstash.es/content/kibana/index.html)

## docker安装nexus私有仓库

### 安装
```shell
# 数据容器
docker run -d --name nexus-data sonatype/nexus echo “data-only container for Nexus”;
# 服务
docker run -d -p 10081:8081 --name nexus --volumes-from nexus-data sonatype/nexus;
```

### 使用
- 登陆http://localhost:10081

