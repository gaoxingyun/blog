---
title: springboot程序监控
date: 2017-07-28 13:50:20
categories:
- Java
tags:
- java
- spring
- 监控
---



#### spring-boot-starter-actuator监控

```
compile('org.springframework.boot:spring-boot-starter-actuator')
```

#### 使用spring-boot-admin进行可视化监控

- spring-boot-admin是一个微服务监控程序，同时也可以在普通springboot程序上使用。

- 开源项目地址[https://github.com/codecentric/spring-boot-admin](https://github.com/codecentric/spring-boot-admin)
```
	compile('de.codecentric:spring-boot-admin-server:1.5.2')
	compile('de.codecentric:spring-boot-admin-server-ui:1.5.2')
	compile('de.codecentric:spring-boot-admin-starter-client:1.5.2')
```

- 配置文件application.yml
```
# 单机版
spring:
 boot:
    admin:
# 服务端监听地址
      context-path: /admin
# 客户端配置 服务器地址 指向本机端口即可实现服务端和客户端集成
      url: http://localhost:${server.port}
```

- springboot配置 AdminConfig.java
```
import org.springframework.context.annotation.Configuration;
import de.codecentric.boot.admin.config.EnableAdminServer;

@Configuration
@EnableAdminServer
public class AdminConfig {

	
}
```