---
title: docker安装oracle数据库
date: 2016-12-07 18:08:45
categories:
- 总结
tags:
- docker
- database
---

# docker安装常用服务

## docker安装
1. 登录docker官网，下载docker安装包，安装
2. Preferences -> File Sharing添加需共享的文件夹

#### 参考资料
[https://www.docker.com](https://www.docker.com)

## docker安装mysql数据库

#### mysql安装
```
docker search mysql
docker pull mysql
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /opt/docker/mysql:/var/lib/mysql  mysql
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
```
docker search oracle
docker pull wnameless/oracle-xe-11g
docker run -d --name oracle -p 11122:22 -p 11521:1521  -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g
```

#### 远程连接
```
ssh root@localhost -p 11122
password: admin
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
```
docker search redis
docker pull redis
docker run --name redis -d  -v  /opt/docker/redis/data:/data -p 6379:6379  redis redis-server --appendonly yes
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
```
docker search rabbitmq:3-management
docker pull rabbitmq:3-management
docker run -d --name rabbitmq --hostname rabbitmq  -e RABBITMQ_DEFAULT_USER=user -e  RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 -p 5672:5672 -v /opt/docker/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management
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

