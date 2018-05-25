---
title: 使用ssh进行网络端口转发
date: 2017-05-20 07:34:10
categories: 
- 工具
tags:
- 工具
---

## 使用ssh进行网络端口转发

### 将外网端口的数据转发的本机

#### 上传本地主机ssh公钥以便无密码ssh登录远程主机

##### 生成ssh公钥证书

- 如果本地主机无.ssh证书
`ssh-keygen -t rsa`

##### 拷贝ssh公钥证书到外网主机

`ssh-copy-id -i .ssh/id_rsa.pub user@外网主机 `

##### 允许外网主机通过ssh公钥免密码登录

- 查看外网主机/etc/ssh/sshd_config文件，确认以下三行未被注释掉

```
RSAAuthentication yes  
PubkeyAuthentication yes  
AuthorizedKeysFile      .ssh/authorized_keys 
```

##### 测试远程免密码登录

- 输入以下命令不需输入密码即可登录外网主机，说明此部分完成
`ssh user@外网主机`


##### 修改外网主机ssh配置
- 默认情况下主机绑定127.0.0.0地址，无法通过外网访问转发的端口

```
vi /etc/ssh/sshd.config
# 在文件中添加 GatewayPorts yes
# 重启 sshd 服务： 
/etc/init.d/sshd restart
```


#### 端口映射

- -f 是后台运行
- -N 是指仅转发端口，不执行命令
- -o ServerAliveInterval=60 代表保持持久连接
- -g 允许非本机地址(任何公网IP)连接远程主机端口.这个选项非常重要, 要让这个选项生效需要在公网主机(ssh server)上编辑/etc/ssh/sshd_config 添加一行GatewayPorts yes 然后保存退出并 service ssh restart
```
ssh -gfNR 9999:localhost:9999 -o ServerAliveInterval=60  user@远程主机
```

