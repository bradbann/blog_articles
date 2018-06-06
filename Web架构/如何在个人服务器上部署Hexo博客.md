---
title: 如何在个人服务器上部署Hexo博客
date: 2018-01-20 15:25:31
tags: Web开发
---

Hexo是github上一个优秀的开源博客框架，支持markdown解析文章，有多种第三方主题插件供选择。
Hexo默认使用github仓库作为资源挂载库，也就是说配置好的Hexo博客是部署在github上的，
你只需要自己购买一个域名解析到对应的github-page就行了。

这样的部署方式虽然比较便捷，但github访问有时候会出现问题，且为了摆脱这种寄人篱下的感觉，
本篇讲介绍如何将博客部署到个人的服务器。

**服务器:** 腾讯云 ubuntu16.04 LTS
**编译环境：** nodeJS 8.9.4

**大致思路:**

1 在云服务器上建立私人git服务器并配置本地仓库

2 在个人开发pc上下载hexo框架源码，并在配置文件中设置push到云服务器上的私人仓库。

3 pc打包上传hexo静态文件并上传到服务器上的私人仓库

4 仓库中设置git钩子将已上传的静态资源拷贝到自定义目录下

5 通过nginx部署web服务，配置静态资源访问目录到自定义目录。

下面是**详细教程**：

**首先安装最新版的nodeJS**

我是直接在官网下载的,[(官网下载地址)](http://nodejs.cn/download/),下载下来后是一个xz格式的压缩包，
通过二次解压后会得到一个带有bin目录的文件夹。将整个文件夹重新命名为node8,然后在~/.bashrc文件末尾添添加环境变量:

    export NODE_HOME=/home/ubuntu/node8
    export PATH=$NODE_HOME/bin:$PATH

配置完毕后记得使用source命令使其生效。服务器和开发PC上都要安装nodeJS编译环境。

**在服务器上建立git服务器并配置博客仓库**

自定义个人仓库地址，这里我选择了当前用户根目录。即/home/ubuntu目下,使用命令:

    mkdir blog.git  
    cd blog.git
    git init --bare

便完成了仓库初始化。

PC上随便找个地方，输入命令 

git clone yourusername@server_ip:~/path/to/your/blog.git

测试下能否克隆成功，能则继续，不行的话将本机的ssh公钥粘贴到服务器的.ssh目录中的authorized_keys文件里。

**PC上配置安装Hexo开发环境**

    npm install hexo-cli -g  #安装hexo构建工具
    hexo init blog #初始化hexo工程
    cd blog #进入工程根目录
    npm install #安装依赖
    hexo server  #启动本地调试服务器
    
然后打开浏览器输入 http://localhost:4000/ 就能看到博客界面了  
![](http://redtreeblog-1253690989.cosgz.myqcloud.com/landscape.png)

若端口已占用，可输入 
    
    hexo s -p 端口号 #切换端口

hexo官网上提供了许多主题，有兴趣的小伙伴自行搜索下载,[地址](https://hexo.io/)。 

下载完后将主题包放到hexo的themes目录下，然后修改_config.yml中theme为对应的包名就可以了。

下面是hexo的常用命令

    hexo s  #开启本地调试
    hexo g  #编译静态资源  
    hexo new 'title'  #生成一篇新的md文件，即博客内容
    hexo d #将静态资源push到仓库
    
**在本地调试完毕后，我们要如何上传资源到服务器上呢？**

修改_config.yml文件,在deploy项中填写:

    deploy:
        type: git
        message: update
        repo: yourusername@server_ip:~/path/to/your/blog.git,master

然后每次上传前，先使用 hexo g 重新打包，再使用 hexo d (可能会提示输入ssh登录密码) 就可以成功上传了。

**上传成功后通过nginx部署hexo资源**

首先修改hooks文件

    cd /path/to/your/blog.git/hooks
    touch post-receive
    vim post-receive
    
使用如下的脚本
    
    #!/bin/bash -l
    GIT_REPO=/home/ubuntu/blog.git
    TMP_GIT_CLONE=/home/ubuntu/tmp/blog
    PUBLIC_WWW=/home/ubuntu/project/blog
    rm -rf ${TMP_GIT_CLONE}
    git clone $GIT_REPO $TMP_GIT_CLONE
    rm -rf ${PUBLIC_WWW}
    cp -rf ${TMP_GIT_CLONE} ${PUBLIC_WWW}

其中public_www就是用于最终存放nginx映射的静态资源文件目录。
通过此脚本，每次仓库接收到新上传的内容时都会自动拷贝资源到该目录下。
tmp目录可能要自己新建。

然后记得更改脚本和各目录的权限

    chmod +x post-receive
    chmod 777 -R /path/to/your/blog
    

**最后便是修改nginx配置咯**

     server {
    listen 80 ;
    root /home/ubuntu/project/blog;
    server_name www.redtreeai.com redtreeai.com;                                 
    access_log  /home/ubuntu/nginxlog/blog_access.log;                           
    error_log   /home/ubuntu/nginxlog/blog_error.log;                            
    location / {                                                                 
        root /home/ubuntu/project/blog;
        if (-f $request_filename) {
            rewrite ^/(.*)$  /$1 break;
        }
    }                                                                            
}

**配置完毕后重启nginx服务就可以实现远程自动部署了，
之后pc端每次打包上传代码后服务器会自动更新资源。**

**喜欢的点个赏吧~**

**(本文原创，转载请注明出处，商业用途请联系博主)**