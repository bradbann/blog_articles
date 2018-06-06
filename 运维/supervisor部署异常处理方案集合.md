---
title: supervisor部署异常处理方案集合
date: 2018-06-06 15:26:02
tags: 运维
---

**错误一：supervisor 进程端口被占用**

Error: Another program is already listening on a port that one of our HTTP servers is configured to use.Shut this program down first before starting supervisord.﻿​

**处理方法**

    sudo unlink /tmp/supervisor.sock    (断开之前的supervisor sock链接)


