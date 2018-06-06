---
title: Nginx 配置及各项功能详解
date: 2018-01-18 10:10:10
tags: 运维
---

**Nginx是啥**

Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。

**什么是反向代理**

反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

**如何安装**

如果您是ubuntu系统,并且不需求nginx的最新特性，那么完全可以通过apt市场进行快速安装。 
安装nginx前，需要先安装相关依赖，包括： 

安装gcc g++的依赖库 
ubuntu平台可以使用如下命令。 

**1 gcc g++ 模块**

    apt-get install build-essential 

    apt-get install libtool 

**2 pcre 依赖** 

    sudo apt-get update 

    sudo apt-get install libpcre3 libpcre3-dev 

**3 zlib 依赖** 

    apt-get install zlib1g-dev 

**4 ssl 依赖库** 

    apt-get install openssl

**然后就是** 
    
    sudo apt-get install nginx

ubuntu会自动帮你完成系统服务的配置，之后可以通过命令: 

**修改nginx配置：**

    sudo vim /etc/nginx/nginx.conf  

**启动服务** 

    service nginx start 

打开浏览器，访问本机ip地址，看到welcome to nginx界面，表示安装成功。

**NGINX主要功能**

**1、静态HTTP服务器**

首先，Nginx是一个HTTP服务器，可以将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端。

    server { 
    listen80; # 端口号 
    location / { 
    root /usr/share/nginx/html; # 静态文件路径 
    } 
    }

**2、反向代理服务器** 
    
    server { 
    listen80; 
    location / { 
    proxy_pass http://192.168.20.1:8080; # 应用服务器HTTP地址 
    } 
    }

**3、负载均衡** 
    
    upstream myapp { 
    server192.168.20.1:8080; # 应用服务器1 
    server192.168.20.2:8080; # 应用服务器2 
    } 
    server { 
    listen80; 
    location / { 
    proxy_pass http://myapp; 
    } 
    } 
    
以上配置会将请求轮询分配到应用服务器，也就是一个客户端的多次请求，有可能会由多台不同的服务器处理。可以通过ip-hash的方式，根据客户端ip地址的hash值将请求分配给固定的某一个服务器处理。 
    
    upstream myapp { 
    ip_hash; # 根据客户端IP地址Hash值将请求分配给固定的一个服务器处理 
    server192.168.20.1:8080; 
    server192.168.20.2:8080; 
    } 
    server { 
    listen80; 
    location / { 
    proxy_pass http://myapp; 
    } 
    } 
    
另外，服务器的硬件配置可能有好有差，想把大部分请求分配给好的服务器，把少量请求分配给差的服务器，可以通过weight来控制。 
    
    upstream myapp { 
    server192.168.20.1:8080weight=3; # 该服务器处理3/4请求 
    server192.168.20.2:8080; # weight默认为1，该服务器处理1/4请求 
    } 
    server { 
    listen80; 
    location / { 
    proxy_pass http://myapp; 
    } 
    } 
**4、虚拟主机** 
有的网站访问量大，需要负载均衡。然而并不是所有网站都如此出色，有的网站，由于访问量太小，需要节省成本，将多个网站部署在同一台服务器上。

例如将www.aaa.com和www.bbb.com两个网站部署在同一台服务器上，两个域名解析到同一个IP地址，但是用户通过两个域名却可以打开两个完全不同的网站，互相不影响，就像访问两个服务器一样，所以叫两个虚拟主机。

    server { 
    listen80default_server; 
    server_name _; 
    return444; # 过滤其他域名的请求，返回444状态码 
    } 
    server { 
    listen80; 
    server_name www.aaa.com; # www.aaa.com域名 
    location / { 
    proxy_pass http://localhost:8080; # 对应端口号8080 
    } 
    } 
    server { 
    listen80; 
    server_name www.bbb.com; # www.bbb.com域名 
    location / { 
    proxy_pass http://localhost:8081; # 对应端口号8081 
    } 
    } 

**简易爬虫过滤方案**

    location / { 
    if ($http_user_agent ~* "python|curl|java|wget|httpclient|okhttp") { 
    return 503; 
    } 
        #正常处理 
    ... 
    } 
这个是在廖雪峰博客上看到的，原文为： 
现在的网络爬虫越来越多，有很多爬虫都是初学者写的，和搜索引擎的爬虫不一样，他们不懂如何控制速度，结果往往大量消耗服务器资源，导致带宽白白浪费了。

其实Nginx可以非常容易地根据User-Agent过滤请求，我们只需要在需要URL入口位置通过一个简单的正则表达式就可以过滤不符合要求的爬虫请求：

变量$http_user_agent是一个可以直接在location中引用的Nginx变量。~*表示不区分大小写的正则匹配，通过python就可以过滤掉80%的Python爬虫

**其他功能**

自行探索吧。

**(本文原创，转载请注明出处，商业用途请联系博主)**
