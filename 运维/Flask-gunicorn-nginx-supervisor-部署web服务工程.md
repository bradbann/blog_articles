---
title: Flask+Gunicorn+Nginx+Supervisor 部署web服务工程
date: 2018-01-18 10:05:42
tags: 运维 
---

服务器系统:ubuntu 16.04 LTS
python环境:3.6

**Flask**
Flask是一个使用python代码编写的轻量级web框架，可以通过简易的配置快速实现各类web服务功能。常用于web_api、个人博客、中小型服务网站搭建。

flask的搭建流程就不细说了，大致如下：
1 通过pip 安装flask 依赖

    pip install flask

2 新建run.py文件 引入flask模块

    flask import Flask,request 
    import json #通常还会引入json模块
    from flask_cors import CORS #跨域访问 cors 按需要配置

    app = Flask(__name__)
    CORS(app, supports_credentials=True) #可去除

    @app.route('/')  
    def hello_world():
         return 'Hello World!'

    @app.route('/test', methods=['GET'])
    def test():  # 导入:
    # 取参数
        text = request.args.get('text')
    
        return json.dumps(text)

    if __name__ == '__main__':
        app.run(debug=True,port=5000) 

然后执行 python run.py 就可以在本地的5000端口看到helloworld界面了。

**Gunicorn** 

这是一个被广泛使用的高性能的Python WSGI UNIX HTTP服务器,也有人选择uWsgi可以达到同样的效果。

首先安装:
    
    pip install gunicorn

然后在flask 配置文件中添加如下代码：


    from werkzeug.contrib.fixers import ProxyFix
        app.wsgi_app = ProxyFix(app.wsgi_app)
      
       

这时候便在flask工程中引入了wsgi代理。gunicorn支持多种worker模式，包括:

1 sync - 默认，使用同步阻塞的网络模型

2 gevent - Requires gevent >= 0.13

3 tornado - Requires tornado >= 0.2
等

本教程选用gevent模式：
    
    pip install gevent

使用下面命令便可通过gunicorn启动flask:
    
    gunicorn -w 4 -b 0.0.0.0:5000 -k gevent run:app

(-w 4 表示启动4个worker -k gevent 表示选择gevent模式)

**Nginx的对应配置**
详见本博客另一篇教程，简单说就是把5000端口映射到nignx代理就行了，就可以实现通过nginx转发http请求。

**supervisor**
服务端运维的重点，系统中各类进程的守护者。由于不支持python3的环境，所以配置会稍微麻烦一点,我们需要通过virtualenv 创建一个python2虚拟环境。

    pip install virtualenv

首先在自定义目录下创建 super 虚拟环境

    virtualenv --distribute -p /usr/bin/python2 super

    cd super
    source bin/activate    #激活虚拟环境
    ./bin/pip install supervisor# 安装supervier
    echo_supervisord_conf > supervisor.conf   # 生成 supervisor 默认配置文件

    #热后便可以通过以下命令对supervisor进行操作：
    supervisord -c supervisor.conf #通过配置文件启动supervisor
    supervisorctl -c supervisor.conf status #察看supervisor的状态
    supervisorctl -c supervisor.conf reload 重新载入 #配置文件
    supervisorctl -c supervisor.conf start [all]|[appname] #启动指定/所有 supervisor管理的程序进程
    supervisorctl -c supervisor.conf stop [all]|[appname] 关闭指定/所有 supervisor管理的程序进程

supervisor.conf中配置对应的gunicorn应用,其他类型的应用配置也类似，比如java的springboot。

    [program:start_gunicorn]
    command=/your/bin/path/to/gunicorn -w 4 -b 0.0.0.0:5000 -k gevent run:app
    directory:/home/ubuntu/project/test1/ #run.py所在的目录
    autostart=true
    redirect_stderr=true
    user=root
    directory=/usr/local/qs-project/web
    stdout_logfile=/var/log/test_test.log

如果要启用supervisor的web端：9001端口.

需要注释掉 配置文件中

[inet_http_server]  的所有子配置项

以及

[supervisorctl] 中的前四项

    deactivate  #退出环境

**至此，Flask的服务端部署教程结束。**

**(本文原创，转载请注明出处，商业用途请联系博主)**