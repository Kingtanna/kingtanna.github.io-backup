---
title: TestNG+Appium+GoCD自动化测试框架搭建
date: 2020-06-01 17:19:58
tags:
---

之前学习了TestNG和GoCD的知识，并且已经利用Appium和Java写了简单的基于page object的mobile测试用例。现在将它们联合起来，搭建一个可使用的自动化测试框架。使用到的工具或者框架如下：
1.Java - 编码语言
2.Appium - mobile测试工具
3.TestNG - 单元测试框架
4.Maven - 项目构建和管理工具
5.GoCD - 持续集成和部署工具

### 一 使用Maven来管理项目
之前在建立测试项目时，使用的是Gradle。由于最近新学习了Maven，所以将其改成Maven项目来巩固Maven的知识。新建一个
Maven项目，将之前的代码拷贝进去。  
<img src ="TestNG-Appium-GoCD自动化测试框架搭建/maven管理项目.png" width=50% >


### 二 添加相关依赖
点击pom.xml文件，然后在工具栏选择code->generate->dependencies，分别添加testng依赖、appium client依赖。这些依赖会自动添加到pom.xml文件中，但是需要手动点击这些依赖右上方的 ```m``` 来真正下载这些依赖。
<img src ="TestNG-Appium-GoCD自动化测试框架搭建/添加依赖1.png" width=50% >   

<img src ="TestNG-Appium-GoCD自动化测试框架搭建/添加依赖2.png" width=50% >

### 三 添加xml文件
由于使用TestNG时需要使用testng.xml文件来运行测试用例，而在建立项目时该文件不会自动生成，因此需要手动去创建该文件。并且添加相应的配置代码。
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Default Suite">
    <test name="Demo">
        <classes>
            <class name="org.example.demo1">
                <methods>

                </methods>
            </class>

        </classes>
    </test>
</suite>
{% endcodeblock %}  


### 四 push测试代码到GitHub仓库
在上述三个步骤做完后，测试代码可以顺利运行。在此基础之上，将本地代码push到远程的GitHub仓库。具体操作步骤，这里不再赘述。

### 五 实现持续集成和部署
1.打开terminal，分别进入到GoCD server和agent目录下，启动server和agent。
2.打开浏览器，输入http://localhost:8153/go/pipelines#!/ ， 进入到GoCD页面。该页面就会提醒新建一个pipeline。那么就按照提示一步步来操作。  
  a.增加material，这里我放的是之前测试代码的仓库地址。可以点击绿色的按钮，测试连接是否成功。
 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline1.png" width=100% >

 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline2.png" width=100% >
 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline3.png" width=100% >
 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline4.png" width=100% >
 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline5.png" width=100% >
 <img src ="TestNG-Appium-GoCD自动化测试框架搭建/automationMobileTesting/CreatePipeline6.png" width=100% >


 


