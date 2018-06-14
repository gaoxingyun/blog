---
title: maven使用
date: 2017-06-23 09:12:31
categories: 
- 工具
tags:
- 工具
- java
---


#### 添加jar包到maven本地仓库

- 添加jar包到maven本地仓库
```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=9.0.2.0.0 -Dpackaging=jar -Dfile=ojdbc14.jar
```

#### 跳过单元测试

- 
```
mvn package -Dmaven.test.skip=true
```


#### 发布项目到私有仓库

1. 在项目Pom.xml中<project></project>之间加入
```xml
<distributionManagement>
    <repository>
        <id>releases</id>
        <name>Popocloud Release Repository</name>
        <url>http://maven:8081/nexus/content/repositories/releases/</url>
     </repository>
    <snapshotRepository>
        <id>snapshots</id>
        <name>Popocloud Snapshot Repository</name>
        <url>http://maven:8081/nexus/content/repositories/snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

2. 确保maven.setting.xml中加入了私有仓库用户名和密码
```xml
<server>  
   <id>releases</id>  
   <username>admin</username>  
   <password>admin123</password>  
 </server>  
 
 <server>
      <id>snapshots</id>
      <username>admin</username>
      <password>admin123</password>
  </server>
</servers>
```

3. 对项目执行maven deploy命令，可实现将项目发布的maven私有仓库供其他开发人员使用。
```
mvn deploy:deploy-file -Dmaven.test.skip=true -Dfile=alipay-sdk-java20151021120052-1.0.jar -DgroupId=alipay -DartifactId=alipay-sdk-java20151021120052 -Dversion=1.0 -Dpackaging=jar -DrepositoryId=nexus-thirdparty -Durl=http://192.168.13.145:10081/nexus/content/repositories/thirdparty/


```

#### 问题

- eclipse中的maven web项目报错：org/codehaus/plexus/archiver/jar/JarArchiver[http://blog.csdn.net/lele2426/article/details/38963071](http://blog.csdn.net/lele2426/article/details/38963071)

#### 名词

- 一方库：本工程中的各模块的相互依赖
- 二方库：公司内部的依赖库，一般指公司内部的其他项目发布的jar包
- 三方库：公司之外的开源库， 比如apache、ibm、google等发布的依赖