---
title: Flask工程结构探讨--无需蓝本的路由分离
date: 2018-01-18 15:31:13
tags: Web开发
---

刚开始学习Flask框架，很多人会觉得简单、便捷。
特别适合做轻量应用开发或者WebAPI服务。
但是若想把工程扩大，就会发现局限性，
比如没有封装好的数据库ORM工具，比如没有统一的工程架构Demo。
其实，深入学习Flask后，便会发觉这些其实也是Flask的优点:
拓展性强、自由度高。

Flask社区的活跃度毋庸质疑，各种第三方工具支持也在保持更新中，
许多杰出程序员都对其贡献了代码。 

也就是说，web框架该有的功能Flask基本都有，只不过需要自己去整合，
需要开发者对web开发模式有一定了解。

本文将介绍一种 Flask框架的 通用架构方式，可以解决以下几个问题：

1 **路由分离**。 （无需通过官方的Blueprint）

2 **服务进程启动时预加载可复用数据至内存中**。

整体架构如下：
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/all.png)

cache: 用于存放数据预加载工具

controller: 分离后的路由控制器

data/log/model: 资料/日志/数据库模型映射

test: 测试集

utils: 工具包

run.py 服务器启动入口

setting.py: 服务基本配置


先讲第一点：**路由分离**

第一步，在setting.py 文件中写入Flask基本配置：
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/setting.png)

第二步,在run.py 文件中写入服务启动代码 包含wsgi代理配置
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/run2.png)
通过socket模块可监控计算机环境，自动区分debug模式和product模式。
此时，要将setting中初始化的app模块引入到本文件中。

第三步,在controller目录中新建 welcome.py
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/welcome.png)
同样要引入setting模块中的app对象,此路由作为HomePage.（request对象则在需要解析http数据时引入）

第四步,setting.py文件中引入对应的控制器文件。

然后运行run.py 文件 ，看到hello world 字样， 便实现了路由分离。

接着讲第二点: **数据预加载**

第一步，在cache目录中新建cache_data.py文件
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/newcache.png)
定义一个用于存储数据集合的对象以及相应的数据获取方法

第二步,在setting.py中引入此文件，调用数据加载方法。

第三步，在controller目录中新建一个test.py文件
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/test.png)
此时便可引用cache目下所有的预加载数据集。

第四步，在setting.py中配置test.py路由

第五步，启动run.py 验证
![](	http://redtreeblog-1253690989.cosgz.myqcloud.com/start%20serve.png)
可以看见终端显示数据已加载

打开浏览器验证test接口:
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/666.png)

看见预加载的数据，功能实现。


**(本文原创，转载请注明出处，商业用途请联系博主)**