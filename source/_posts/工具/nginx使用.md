---
title: nginx使用
date: 2017-06-16 14:44:30
categories: 
- 工具
tags:
- 工具
- 并发
- 分布式
---

## nginx使用

- [http://nginx.org/](http://nginx.org/)

#### nginx安装

- [http://www.cnblogs.com/taiyonghai/p/6728707.html](http://www.cnblogs.com/taiyonghai/p/6728707.html)

#### nginx配置文件

###### 文件路径

安装完成nginx后，输入nginx -h命令，可在-p prefix看到nginx文件路径，可在-c filename看到nginx配置文件位置。

```
[root@iZuf69lk5j1zbk79emci6eZ nginx]# nginx -h
nginx version: nginx/1.10.2
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/share/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```

###### nginx命令

```
nginx -h 帮助
nginx -s singnal 最常用命令，向nginx发送信号 singnal可为：stop, quit, reopen, reload。
```

###### nginx 内部变量

1. $remote_addr 与$http_x_forwarded_for 用以记录客户端的ip地址； 
2. $remote_user ：用来记录客户端用户名称； 
3. $time_local ： 用来记录访问时间与时区；
4. $request ： 用来记录请求的url与http协议；
5. $status ： 用来记录请求状态；成功是200， 
6. $body_bytes_sent ：记录发送给客户端文件主体内容大小；
7. $http_referer ：用来记录从那个页面链接访问过来的； 
8. $http_user_agent ：记录客户端浏览器的相关信息；


###### nginx uri匹配原则

- 正则 location 匹配让步普通 location 的严格精确匹配结果；但覆盖普通 location 的最大前缀匹配结果。
- 普通 location 是最大前缀匹配原则，例如会匹配到/a 而不会匹配到/ 。
- 匹配完成普通 location 后，不再匹配正则 location 的方法：
   1. 当普通 location 前面指定了“ ^~ ”，特别告诉 Nginx 本条普通 location 一旦匹配上，则不需要继续正则匹配；
   2. 当普通location 恰好严格匹配上，不是最大前缀匹配，则不再继续匹配正则。

##### nginx反向代理配置

```
server {  
    listen       80;  #监听端口
    server_name  10.0.0.136; #根据环境介绍，nginx server ip  

    location /a {  #表示匹配所有地址a开头的地址，但是遵循最大前缀原则
           proxy_pass http://127.0.0.1:8080/; #被代理的服务器地址端口  
         }
    location = /b { #存在=代表仅仅匹配/b一个地址
           deny  all; #拒绝地址
    }
    localtion ~ \.js$ { #~表示匹配的是一个正则表达式(区分大小写)，此处意义为以.js结尾的地址（.在正则中需转义）
        proxy_pass http://127.0.0.1:8080/;
    }
    localtion ~* \.css$ { #~*表示匹配的是一个正则表达式(不区分大小写)，此处意义为以.css结尾的地址（.在正则中需转义）
        proxy_pass http://127.0.0.1:8080/;
        deny 127.0.0.1;  #拒绝的ip
        allow 127.0.0.1; #允许的ip
    }
    
}
```

##### nginx配置多个证书

- 访问a.xingyung.top，使用a证书，访问b.xingyung.top，使用b证书。
```
server {  
    listen       443;
    server_name  a.xingyung.top;
    ssl                  on;  
    #证书  
    ssl_certificate      /data/key/a.crt;  
    ssl_certificate_key  /data/key/a.key;  
    ssl_session_timeout  5m;  
    ssl_protocols  SSLv2 SSLv3 TLSv1;  
    ssl_ciphers  HIGH:!aNULL:!MD5;  
    ssl_prefer_server_ciphers   on;  
    location / {  
        proxy_pass http://172.16.8.8:8080/;         
        proxy_redirect     off;           
        proxy_set_header   Host             $host;            
        proxy_set_header   X-Real-IP        $remote_addr;              
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   Content-Type     $content_type;              
        client_max_body_size       100m;              
        index  index.html index.htm;  
    }      
}

server {  
    listen       443;
    server_name  b.xingyung.top;
    ssl                  on;  
    #证书  
    ssl_certificate      /data/key/b.crt;  
    ssl_certificate_key  /data/key/b.key;  
    ssl_session_timeout  5m;  
    ssl_protocols  SSLv2 SSLv3 TLSv1;  
    ssl_ciphers  HIGH:!aNULL:!MD5;  
    ssl_prefer_server_ciphers   on;  
    location / {  
        proxy_pass http://172.16.8.9:8080/;         
        proxy_redirect     off;           
        proxy_set_header   Host             $host;            
        proxy_set_header   X-Real-IP        $remote_addr;              
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;              
        client_max_body_size       100m;              
        index  index.html index.htm;  
    }      
}



```


##### nginx负载均衡配置

##### nginx https ssl配置

##### nginx常用变量

[http://nginx.org/en/docs/varindex.html](http://nginx.org/en/docs/varindex.html)

##### nginx跨域设置
- 可以通过nginx反向代理实现跨域访问，这也是一种超简单的跨域方式，需要添加以下几个头
```
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Credentials' 'true';
add_header 'Access-Control-Allow-Methods' '*';
```


##### nginx代理本地html目录配置

```
# 将需代理目录所属用户加入到nginx.conf下，否则会报(13: Permission denied)
user nginx pmweb;

# 代理配置, 会访问 /home/user/html/index.html文件。
location  /html {
               root /home/user;
               index index.html;
             }
```


#### 问题

- nginx重试机制导致前端发送一次支付请求，后端收到两次支付请求，请求重复提交，客户重复支付的问题。
```
Syntax:	proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | http_503 | http_504 | http_403 | http_404 | http_429 | non_idempotent | off ...;
Default:	
proxy_next_upstream error timeout;
Context:	http, server, location
```
默认情况下，后端服务超时，nginx会发送请求到下一个服务，对于无幂等性的服务，会造成服务的异常情况，所以对于支付等服务，要设置proxy_next_upstream off
```
Syntax:	proxy_read_timeout time;
Default:	
proxy_read_timeout 60s;
Context:	http, server, location
```
proxy_read_timeout，代理超时时间，默认为60s。

[http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)