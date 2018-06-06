---
title: 实时日志监控工具Log.IO 的配置流程
date: 2018-01-18 10:23:11
tags: 运维
---

![](http://logio.org/screen_detail.jpg)
[官网地址](http://logio.org/)

这是一款基于node.js 和 socket.io 构建的web端日志实时监控工具，搭建迅速，界面有逼格。

下面是ubuntu16.04上的安装流程:

 

**安装 nodejs**    

$ sudo apt install nodejs-legacy   (建议去官网下载最新版本)

**2 安装 node npm**   
 
$ sudo apt install npm 

**3 安装 log.io**    

$ sudo npm install -g log.io --user "ubuntu"   #这边填写自己对应的用户名      

**4  修改配置文件**    

   vim ~/.log.io/harvester.conf

    exports.config = {
    nodeName: "填写监控应用的名称",
    logStreams: {     #监控的io流
    sys_io: [     #每个流里对应多个监控节点，可手动添加，记得加逗号
    "/home/yourpate/app.log"
    ]
    },
    server: {
    host: '0.0.0.0',
    port: 28777  #默认端口配置
    }
    }

**5启动服务端**   

log.io-server

**6启动客服端**   

log.io-harvester  #加载配置文件

然后打开浏览器访问本地28778端口就可以看到监控画面啦，具体的UI操作就自己研究吧

**7友情提示**
服务挂死重启时，要先启动服务端再启动客户端，才能正常加载配置和读取数据。

**(本文原创，转载请注明出处，商业用途请联系博主)**