---
title: grafana+prometheus+node-exporter 搭建服务器全局健康指标监控
date: 2018-01-17 10:35:19
tags: 运维
---

**第一步 安装 grafana 版本请在官网下载最新版本**

1、首先从官网上下载相应的包，(网址)[http://grafana.org/download]

2、安装

    cd Downloads

    sudo dpkg -i grafana_4.6.2-1486989747_amd64.deb

（安装过程中按照系统提示操作即可，需要创建基础服务文件）

3、运行

    sudo /bin/systemctl start grafana-server

4、登录web

http://localhost:3000

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/grafana.png)
 

**第二步 安装prometheus** 

官网下载最新版本 2.0.0 tar.gz

解压后直接./运行 prometheus 就行

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/prometheus.png)

默认端口 9090
 

**第三步 node-exporter数据采集插件**

官网下载最新版本后直接解压复制到 prometheus 文件根目录下，运行./node-exporter

默认端口 9100

**第四步 prometheus  配置 target 采集 node-expoter**

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/targets.png) 

**第五步 grafana 添加prometheus 数据采集源并增加显示相应仪表盘**

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/grafanaset.png)

![](http://redtreeblog-1253690989.cosgz.myqcloud.com/yunwei/grafanapan.jpeg)


