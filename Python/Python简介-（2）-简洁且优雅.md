---
title: Python简介之二 简洁且优雅
date: 2018-01-18 14:24:09
tags: Python
---

Python 的代码写起来十分简洁，做个比喻：同样是实现一项功能，C可能可能需要500行，Java可能需要100行，而Python只要10行。这个比喻有点夸张成分，因为各类程序在代码重构、优化过后，都可以达到很精简的状态（排除复杂的算法和多层嵌套），只不过Python的表达方式更为多样，开放性强。

其实有些编程语言是可以写成一行执行的，但代码堆积在一行可读性极差，也不是真正意义上的简洁，上面提到的行数其实指代码量。另外，Python的编程语法虽然舍弃了诸多复杂的符号表示，但是，取而代之的是Python代码要求采用严格空格缩进表示，同一层级执行的代码需要严格排列对齐，这可能会让初用Python的程序员有点不适，但久而久之便会察觉，Python写的代码规范感十足，格式工整。

下面是我整理的部分Python代码格式范例(V3.6)，充分展现出了Python语言的优雅特性：

**1 双变量值交换**
许多语言需要引入tmp缓冲变量完成此操作：
    
    a=1;
    b=2;
    tmp =a ;
    a =b ;
    b =tmp;

而Python中可以这样写：

    a=1
    b=2
    a,b=b,a

**2循环遍历数组元素**
某些语言：

    ListA = [1,2,3]
    for (int n =0;n<ListA.length(),n++){
    sout(ListA[n])
    }

或者 
    
    ListA = [1,2,3]
    for (int single:ListA){
    sout(single)
    }

**在Python只需要：**

    ListA= [1,2,3]
    for single in ListA:
    print single

**3 数组字符串元素拼接**
    
    cars = ['BMW','Benz','DasAuto']
    resutl_str=''
    for car in cars:
       result_str = result_str + car

更优雅的写法：

    cars = ['BMW','Benz','DasAuto']
    result_str = ','.join(cars) 

Join写法比‘+’写法更省内存，因为后者每次拼接都需要在内存中生成一个新的对象。

**4 文件流IO**

    file = open('name.txt')
    data=file.readlines()
    file.close

更优雅的写法：
    
    with open ('name.txt') as file:
        data=file.readlines()  #with写法可以自动关闭IO流

**5 字典 Key,Value 提取**
方法1：

    for key in dict:
        print(key,dict[key])
方法2：
    
    for key,value in dict.items():
        print(key,value)

两种方法各有优劣，前者速度较慢，因为每次获取value都需要重新根据key进行hash查找。后者则消耗内存更大，当字典较为庞大时，可能使得内存消耗翻倍。

**本章到此结束，下一篇将介绍Python开发的常用库以及Python的应用领域，敬请期待！**

**(本文原创，转载请注明出处，商业用途请联系博主)**

