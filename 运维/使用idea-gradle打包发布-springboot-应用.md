---
title: 使用idea+gradle打包发布 springboot 应用
date: 2018-01-17 10:36:01
tags: Web开发
---

gradle配置文件如下:

    apply plugin: 'java'
    apply plugin: 'idea'﻿​

    repositories {
    mavenCentral()
    }

    jar {
    String someString = ''
    configurations.runtime.each {someString = someString + " lib//"+it.name}
    manifest {
    attributes 'Main-Class': 'com.abc.dfg'
    attributes 'Class-Path': someString
    }
    }
    //清除上次的编译过的文件
    task clearPj(type:Delete){
    delete 'build','target'
    }
    task copyJar(type:Copy){
    from configurations.runtime
    into ('build/libs/lib')
    }
    //把JAR复制到目标目录
    task release(type: Copy,dependsOn: [build,copyJar]) {
    // from 'conf'
    // into ('build/libs/eachend/conf') // 目标位置 
    }

 

**注意 mainclass方法需要修改**
**应用发布到服务器上需要拷贝lib文件夹**