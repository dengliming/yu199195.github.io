---
title: tcc源码解析系列之tcc-admin事务管理后台
date: 2017-10-13 19:04:53
categories: hmily-tcc
permalink: tcc/tcc-seven
---

### Tcc-admin 启动教程
启动前提：分布式事务项目已经部署并且运行起来，用户根据自己的RPC框架进行使用
[dubbo 用户](https://github.com/yu199195/hmily/wiki/dubbo%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97)
 [springcloud 用户](https://github.com/yu199195/hmily/wiki/springcloud%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97)


* 首先用户使用的JDK必须是1.8+  本地安装了git ,maven ，执行以下命令

```
git clone https://github.com/yu199195/hmily.git

maven clean install
```

* 使用你的开发工具打开项目，比如idea Eclipse

### 步揍一：  配置并且启动Tcc-admin
* 在项目中的application.properties文件中，修改您的服务端口，redis配置等配置:

```java
#admin项目的tomcat端口
#服务端口
server.port=8888
server.context-path=/tcc-admin
server.address=0.0.0.0
spring.application.name=tcc-admin


# admin项目激活的类型，支持db，file，mongo，zookeeper，redis 下文会继续讲解
spring.profiles.active=db

# admin管理后台的用户名，用户可以自己更改
tcc.admin.userName=admin
# admin管理后台的密码，用户可以自己更改
tcc.admin.password=admin


#采用二阶段提交项目的应用名称集合，这个必须要填写
tcc.application.list=account-service,inventory-service,order-service

#事务补偿的序列方式
compensation.serializer.support=kryo

#事务补偿最大重试次数
compensation.retry.max=10

#dbSuport
compensation.db.driver=com.mysql.jdbc.Driver
compensation.db.url=jdbc:mysql://192.168.1.68:3306/tcc?useUnicode=true&amp;characterEncoding=utf8
compensation.db.username=xiaoyu
compensation.db.password=Wgj@555888

#redis
compensation.redis.cluster=false
compensation.redis.hostName=192.168.1.68
compensation.redis.port=6379
compensation.redis.password=
#compensate.redis.clusterUrl=127.0.0.1:70001;127.0.1:7002

#mongo
compensation.mongo.url=192.168.1.68:27017
compensation.mongo.dbName=happylife
compensation.mongo.userName=xiaoyu
compensation.mongo.password=123456

#zookeeper
compensation.zookeeper.host=192.168.1.116:2181
compensation.zookeeper.sessionTimeOut=200000

```

## 配置解释



* 关于 tcc.application.list配置：这里需要配置每个参与Tcc分布式事务的系统模块的applicationName，多个模块用 "," 分隔，这里必须要配置。

* 关于 compensation.serializer.support 配置，这里是指参与Tcc分布式事务系统中，配置事务补偿信息的序列化方式。

* 关于 spring.profiles.active 配置 admin项目激活的类型，支持db，file，mongo，zookeeper，
  这里是指参与Tcc分布式事务系统中，配置事务补偿信息存储方式，如果您用db存储，那这里就配置成db，同时配置好db等信息。 其他方式同理。 注意，每个模块请使用相同的序列化方式和存储类型

* 关于 tcc.admin 等配置。 这里就是管理后台登录的用户与密码，用户可以进行自定义更改。


### 步揍二：修改本项目static 文件夹下的 index.html

```html
<!--href 修改成你的ip 端口-->
<a id="serverIpAddress" style="display: none" href="http://192.168.1.132:8888/admin">
```


### 步揍三: 运行 AdminApplication 中的main方法。


### 步揍四:在浏览器访问  http://ip:prot/tcc-admin/index.html  ,输入用户名，密码登录。


![登录界面](https://yu199195.github.io/images/hmily-tcc/tccLogin.png)

![事务补偿](https://yu199195.github.io/images/hmily-tcc/tccCompensation.png)






### 如有任何问题欢迎加入QQ群：162614487 进行讨论
