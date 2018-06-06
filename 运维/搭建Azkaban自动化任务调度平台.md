---
title: 搭建Azkaban自动化任务调度平台
date: 2018-01-17 17:06:57
tags: 运维
---

环境：ubuntu 16.04

配置版本 azkaban 2.5.0

下载三个压缩包，分别为：

1 azkaban-web-server-2.5.0 .tar.gz

2 azkaban-executor-server-2.5.0.tar.gz

3 azkaban-sql-script-2.5.0.tar.gz

自定义目录下新建一个叫做azkaban的文件夹，将三个压缩包置入后解压。

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/az01.png)

分别修改对应的解压文件夹命名：webserver executor sql 
下载包资源请在网上自寻(需要的小伙伴也可在文章下方留言，带上邮箱地址)

然后配置mysql :

新建用户azkaban并赋予所有权限

新建名为azkaban的数据库

cd 到 sql文件夹下，使用azkaban用户登录mysql / azkaban数据库

执行命令 source create-all-sql-2.5.0.sql 创建表

然后分别进入webserver /conf 配置 azkaban.properties:

    Azkaban Personalization Settings
    
    azkaban.name=KeekoAI 
    azkaban.label=My Local Azkaban 
    azkaban.color=#FF3601 
    azkaban.default.servlet.path=/index 
    web.resource.dir=web/ 
    default.timezone.id=Asia/Shanghai
    
    Azkaban UserManager class
    
    user.manager.class=azkaban.user.XmlUserManager 
    user.manager.xml.file=conf/azkaban-users.xml
    
    Loader for projects
    
    executor.global.properties=conf/global.properties 
    azkaban.project.dir=projects
    
    database.type=mysql 
    mysql.port=3306 
    mysql.host=localhost 
    mysql.database=azkaban 
    mysql.user=azkaban 
    mysql.password=azkaban!@# 
    mysql.numconnections=100
    
    Velocity dev mode
    
    velocity.dev.mode=false
    
    Azkaban Jetty server properties.
    
    jetty.maxThreads=25 
    jetty.ssl.port=8443 
    jetty.port=8081 
    jetty.keystore=keystore 
    jetty.password=123456 
    jetty.keypassword=123456 
    jetty.truststore=keystore 
    jetty.trustpassword=123456
    
    Azkaban Executor settings
    
    executor.port=12321
    
    mail settings
    
    mail.sender= 
    mail.host= 
    job.failure.email= 
    job.success.email=
    
    lockdown.create.projects=false
    
    cache.directory=cache

配置 azkaban-users.xml : (匹配自己的密码)


在webserver 中 生成keystore

keytool -keystore keystore -alias jetty -genkey -keyalg RSA 密码与jetty设置中一样

进入executor 配置 azkaban.properties:

    Azkaban
    
    default.timezone.id=Asia/Shanghai
    
    Azkaban JobTypes Plugins
    
    azkaban.jobtype.plugin.dir=plugins/jobtypes
    
    Loader for projects
    
    executor.global.properties=conf/global.properties 
    azkaban.project.dir=projects
    
    database.type=mysql 
    mysql.port=3306 
    mysql.host=localhost 
    mysql.database=azkaban 
    mysql.user=azkaban 
    mysql.password=azkaban!@# 
    mysql.numconnections=100
    
    Azkaban Executor settings
    
    executor.maxThreads=50 
    executor.port=12321 
    executor.flow.threads=30


配置完毕。

启动方式：

分别在webserver 和 executor中的bin目录下启动对应的start.sh

             bin/azkaban-web-start.sh &
             bin/azkaban-executor-start.sh &

然后浏览器输入  https://localhoat:8443
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/azkaban.png)