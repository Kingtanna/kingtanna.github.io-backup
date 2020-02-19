---
title: Gatling学习笔记1
date: 2020-02-17 19:51:06
tags:
---

Gatling是一款基于Scala开发的高性能服务器性能测试工具，它主要用于对服务器进行负载等测试，并分析和测量服务器的各种性能指标。Gatling主要用于测量基于HTTP的服务器，比如Web应用程序，RESTful服务等。
<!--more-->
### **获取Gatling**
1.官网下载Gatling
[https://gatling.io/open-source/](gatling)  
2.下载好后解压，会得到如下几个文件夹:  

<img src="Gatling学习笔记1/8.png" width="300" height="200">

各个文件夹的作用如下，其中target文件夹会在运行测试后才产生。  

![avatar](Gatling学习笔记1/9.png)

### **使用Gatling进行测试**
1.首先利用自带的脚本来运行测试，从而熟悉整个测试流程。打开命令行窗口并进入Gatling的bin文件夹目录下。  

![avatar](Gatling学习笔记1/1.png)  

2.执行命令gatling.bat，命令行窗口会显示出所有的测试用例  

![avatar](Gatling学习笔记1/2.png)

3.输入0，连续两次按下Enter键，运行第一个测试用例。  

![avatar](Gatling学习笔记1/3.png)

4.运行一段时间后会得到运行结果  

![avatar](Gatling学习笔记1/4.png)  

5.在文件夹results里面也会生成详细的运行结果报告。  

![avatar](Gatling学习笔记1/5.png)
![avatar](Gatling学习笔记1/6.png)






