---
title: Python简介之三 全能与潮流
date: 2018-01-18 14:28:05
tags: Python
---

2017年最新IEEE世界编程语言排行榜公布，Python高居榜首。
在此之前，Python被广泛应用在web开发、系统运维、数据爬虫、数据分析和游戏服务器开发等,随着人工智能的浪潮，Python以其独特的语法特性和丰富的第三方类库成为了机器学习算法编程的优选语言之一。
著名的机器学习框架TensorFlow、Keras等都有Python的对应支持库。

机器学习相关的教程将会在我的另一个栏目中详细介绍。
下面主要介绍下Python常用的开发工具包以及对应类型的优秀产品。

**web开发：**
Flask、Django、Tornado

1 Flask 基于Werkzeug和Jinjia2开发，是一个轻量型web框架，适用于中小型项目、个人博客和web_api服务，可拓展性强。
2 Django 适用于大型服务系统、后台管理系统、OA、ERP等，插件齐全，模块规范，权限控制强大，社区资料也很完善，是一个很成熟的web框架。对了，Django的错误提示界面是笔者见过最好看的   
3 Tornado 这个框架的特点是自带异步非堵塞网络模型，适用于开发TCP Server。
**著名Python web项目：**

1 Reddit 一个国外知名的社交分享网站。最早使用Lisp开发,2005年改为Python。
2 豆瓣网 国内著名的文化产品资料数据库网站和文化交流平台。
3 Youtube 著名的视频分析网站，类似国内的bilibili。

**#游戏开发:**
Pygame、Cocos2d-Python

1 Pygame是最经典的python游戏开发工具，适合新手入门。
2 Cocos2d-Python是著名开源游戏框架cocos2d的python支持包，其设计理念较为新颖。
**著名Python开发的游戏或相关模块:**

1 席德梅尔的文明4 举世闻名的策略游戏，世界核平梗的起源。
2 战地2 一款射击类游戏，顶尖的武器系统，自由度高。
3 EVE 大型太空经营管理、战斗策略网游。

**数据库相关：**
SQLAIchemy、Pymysql

1 Pymysql 是python自带的数据库管理工具，连接较为简便，但操作数据库时需要自己编写sql语句。
2 SQLAIchemy 是一个第三方工具，通过ORM方式管理数据库，适用于大型项目的数据库业务。
**著名的Python编写的数据库管理工具:**

MySQL Workbench  一款可视化数据库管理工具，类似Navicat。

**Http数据处理及数据爬虫工具:**
Requests、Urllib、Scrapy、BeautifulSoup

1 BeautifulSoup 是一个可以从html或xml文件中提取数据的python库，可单独作为一个模块灵活运用到各类框架中。
2 Scrapy 是一个完整的爬虫系统框架，功能齐全。
3 Urllib是python的标准库，如果只是单页面解析或接口数据解析，使用此工具较为便捷。
4 Requests 同理Urllib,适合处理普通的Http协议数据和web_api服务数据解析。

**GUI图形界面编程：**
建议初学者使用TKinter,了解下GUI编程的基本概念。熟练掌握后可用于开发PC端的各类可视化应用，或者制作一些静态加载类型桌面游戏（比如五子棋）。

**图像处理工具：**
PIL、OpenCV2

1 PIL(pillow 为最新更新包，PIL已停止更新) 为Python的图像处理标准库，可完成常规的图像处理。
2 OpenCV 为专业图像处理库，对c++、python等语言均有支持，可以说是PIL的全面升级版，同时使用OpenCV还可以进行图像识别开发，如人脸识别技术等。

**系统运维及相关软件开发：**
Python由于具有脚本语言特性，很多时候用来编写进程管理程序比使用Bash相关脚本来得方便。著名的Linux后台进程管理神器 Supervisor便是Python2的杰作。

**自动化测试与行为模拟脚本：**
Selenium、Pywin32、Virtkey

1 Selenium作为是一款跨平台自动化测试工具，可以模拟用户在浏览器中的各类行为，常用来测试web应用程序。
2 Pywin32与Virkey分别是针对windows和linux是系统的鼠标、按键模拟操作库，可实现类似按键精灵的功能。

**大数据分布式集群框架:**
说到大数据基本上想到的都是Hadoop、Spark这两个工具，然而由豆瓣开发的Dpark也成功地为python的大数据处理模块添砖加瓦，Dpark是Spark的克隆版。

**图像绘制与数据可视化:**
matplotlib、wordcloud、turtle

1 matplotlib是python的图像绘制标准类库，可用于绘制函数曲线、点阵、拓扑图等。
2 wordcloud是第三方库，用于大数据分析的热频词可视化，使得数据分析的结果展示更有逼格。
3 turtle 是一个有趣绘图工具，turtle顾名思义是一只小海龟，用户编制程序然后让小海龟绘制图像，并能展示出绘制过程，特别适合锻炼编程思维。turtle的设计源于早期的LOGO语言编程，常用于儿童编程思维教程。

**机器学习与科学计算：**
NumPy、Tensorflow、Keras

1 NumPy 是 python的数学计算拓展库，适用于矩阵处理和数值编程，并且内置了很多数学函数。
2 Tensorflow Google公司开发的机械学习框架，目前已开源，Tensorflow重新定义了机器学习模型的设计思维，拥有一套独立的语法。学习Tensorflow可以致力于多方面研究，包含自然语言处理、人机博弈、图像识别、自动驾驶、文艺创作等。
3 Keras 是一个深度学习库，基于Tensorflow、Theano、Cntk的部分模块开发，旨在为用户提供友好的机器学习实践体验。

**机器学习领域的优秀作品：**
自然语言处理工具:Jieba、NLTK、StanfordNLP、Word2Vectory等（这些工具的使用会在之后的教程中详细介绍）

各类聊天机器人：微软小冰、小黄鸡、Siri、Cortana、Tay(因学习不正当言论现已下架)、图灵、科大讯飞聊天机器人等。
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/0.jpeg)

**绘画辅助:** 
Google AutoDraw (识别用户画的图并给出对象预测) 
Edges2Cats(基于Tensorflow开发，可将图像拟化成猫，也可拓展其他类别) 
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/1.jpeg)

Image2txt (Tensorflow的教案项目之一，可以对图片进行标记和内容描述)

**游戏辅助、外挂：**
flappy-bird-master (人工智能玩Flappy)
wechat-jump-python(最近很火的微信跳一跳外挂) 
Alphago(不用说了吧) 
OpenAI(Dota2打败世界关键的那个 )

自动驾驶：自行观看去年的百度世界大会。

文艺创作：
PainsChainer (一款漫画自动上色软件)
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/2.jpeg)

另外知乎上有一篇利用Keras转换图像风格的文章有兴趣的小伙伴也可自行搜索。

本篇介绍到此结束。感兴趣的小伙伴们点个赞吧～！心动的小伙伴们赶紧操起键盘敲代码吧！

**(本文原创，转载请注明出处，商业用途请联系博主)**