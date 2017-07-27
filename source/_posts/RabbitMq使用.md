---
title: RabbitMq使用
date: 2017-07-25 17:40:14
categories: 
- 工具
tags:
- mq
---

#### 安装

###### 使用docker安装

```
docker run -d --name rabbitmq --hostname rabbitmq  -e RABBITMQ_DEFAULT_USER=user -e  RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 -p 5672:5672 -v /opt/docker/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management
```

###### 打开管理页面
```
http://localhost:15672
```

###### 使用mq
```
localhost:5672
```

#### 概念

- Broker：简单来说就是消息队列服务器实体。
- Exchange：消息交换机，它指定消息按什么规则，路由到哪个队列。
- Queue：消息队列载体，每个消息都会被投入到一个或多个队列。
- Binding：绑定，它的作用就是把exchange和queue按照路由规则绑定起来。
- Routing Key：路由关键字，exchange根据这个关键字进行消息投递。
- vhost：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离。
- producer：消息生产者，就是投递消息的程序。
- consumer：消息消费者，就是接受消息的程序。
- channel：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务。


#### 博客

- [springboot使用示例](http://www.jianshu.com/p/e1258c004314)


#### 插件

- http://www.rabbitmq.com/community-plugins.html

###### 常用插件

- 延时消息插件  https://bintray.com/rabbitmq/community-plugins/rabbitmq_delayed_message_exchange/v3.6.x#files

###### 插件操作

- `rabbitmq-plugins enable rabbitmq_management`       启用插件
- `rabbitmq-plugins disable rabbitmq_management`      禁用插件
- `rabbitmq-plugins list`                             查看插件列表

