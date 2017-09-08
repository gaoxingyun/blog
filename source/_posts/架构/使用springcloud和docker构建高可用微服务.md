---
title: 使用springcloud和docker构建高可用微服务
date: 2016-12-15 15:59:54
categories: 
- 架构
tags:
- 架构
- java
---

#### Eureka服务注册中心

##### gradle配置
- build.gradle
```gradle
dependencyManagement {
	imports {
		mavenBom ('org.springframework.cloud:spring-cloud-dependencies:Camden.SR3')
	}
}

dependencies {
	compile ('org.springframework.cloud:spring-cloud-starter-eureka-server')
	testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

##### 启动代码
- 只需添加@EnableEurekaServer注解即可配置一个工程为Eureka服务端
```java
@EnableEurekaServer
@SpringBootApplication
public class XyEurekaApplication {

	public static void main(String[] args) {
		SpringApplication.run(XyEurekaApplication.class, args);
	}
}
```

##### application配置
- 需要配置程序自身不作为客户端注册服务
- application.yml
```yml
server:
  port: 10000
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```
##### 登录Eureka管理端
- 登录http://localhost:10000/eureka/即可查看注册到Eureka的服务


