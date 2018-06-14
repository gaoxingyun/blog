---
title: tomcat使用
date: 2017-06-19 09:21:33
categories: 
- 工具
tags:
- 工具
- java
---

###### tomcat配置http强制跳转https

- server.xml 配置https
```
   <Connector  port="9080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="9443" />
    <Connector port="9443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" keystoreFile="key/tomcat.keystore" keystorePass="123456" />
```

- web.xml 配置强制跳转
```
<!-- 在<welcome-file-list>下面添加 -->
<security-constraint>
    <web-resource-collection>
        <web-resource-name>OPENSSL</web-resource-name>
        <url-pattern>/*</url-pattern>
    </web-resource-collection>
    <user-data-constraint>
        <transport-guarantee>CONFIDENTIAL</transport-guarantee>
    </user-data-constraint>
</security-constraint>
```

#### tomcat调优

##### tomcat集群部署

###### session共享方案

- 复制session
- 共享session
- session粘滞

##### server.xml配置

###### Connection配置
1. 指定使用NIO模型来接受HTTP请求 
protocol="org.apache.coyote.http11.Http11NioProtocol" 指定使用NIO模型来接受HTTP请求。默认是BlockingIO，配置为protocol="HTTP/1.1" 
acceptorThreadCount="2"	使用NIO模型时接收线程的数目 

2. 指定使用线程池来处理HTTP请求 
首先要配置一个线程池来处理请求（与Connector是平级的，多个Connector可以使用同一个线程池来处理请求）
``` 
<Executor name="tomcatThreadPool" namePrefix="catalina-exec-" 
maxThreads="1000" minSpareThreads="50" maxIdleTime="600000"/> 
<Connector port="8080"	
executor="tomcatThreadPool"	指定使用的线程池 
```

3. 指定BlockingIO模式下的处理线程数目 
- maxThreads="150"//Tomcat使用线程来处理接收的每个请求。这个值表示Tomcat可创建的最大的线程数。默认值200。可以根据机器的时期性能和内存大小调整，一般可以在400-500。最大可以在800左右。 
- minSpareThreads="25"---Tomcat初始化时创建的线程数。默认值4。如果当前没有空闲线程，且没有超过maxThreads，一次性创建的空闲线程数量。Tomcat初始化时创建的线程数量也由此值设置。 
- maxSpareThreads="75"--一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。默认值50。一旦创建的线程超过此数值，Tomcat会关闭不再需要的线程。线程数可以大致上用 “同时在线人数*每秒用户操作次数*系统平均操作时间” 来计算。 
- acceptCount="100"----指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理。默认值10。如果当前可用线程数为0，则将请求放入处理队列中。这个值限定了请求队列的大小，超过这个数值的请求将不予处理。 
- connectionTimeout="20000" --网络连接超时，默认值20000，单位：毫秒。设置为0表示永不超时，这样设置有隐患的。通常可设置为30000毫秒。 

4. 其它常用设置 
- maxHttpHeaderSize="8192"	http请求头信息的最大程度，超过此长度的部分不予处理。一般8K。 
- URIEncoding="UTF-8"	指定Tomcat容器的URL编码格式。 
- disableUploadTimeout="true"	上传时是否使用超时机制 
- enableLookups="false"--是否反查域名，默认值为true。为了提高处理能力，应设置为false 
- compression="on"   打开压缩功能 
- compressionMinSize="10240"	启用压缩的输出内容大小，默认为2KB 
- noCompressionUserAgents="gozilla, traviata"   对于以下的浏览器，不启用压缩 
- compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain" 哪些资源类型需要压缩 