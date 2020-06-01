---
title: 基于MAC的Appium Desktop环境搭建
date: 2020-02-25 20:30:22
tags: Appium
---
appium是一款开源的移动端自动化测试框架, appium可以测试Android或者iOS平台的原生应用，web浏览器应用以及混合应用。appium在设计时认为不应该让移动端自动化测试限定在某种语言和某个具体的框架中，也就是说任何人都可以使用自己最熟悉最顺手的语言以及框架来做移动端自动化测试。
<!--more-->
因此appium选择了client-server的设计模式。只要client能够发送http请求给server，那么client用任何语言来实现都是可以的，因此appium做到了支持多语言。其次appium扩展了webdriver的协议，没有重新去实现一套webdriver。这样的好处是以前的webdriver api能够直接被继承过来，各种语言的binding都可以拿来就用，省去了为每种语言开发一个client的工作量。

appium的核心其实是一个暴露了一系列REST API的server。这个server的功能其实很简单：监听一个端口，然后接收由client发送来的command。翻译这些command，把这些command转成移动设备可以理解的形式发送给移动设备，然后移动设备执行完这些command后把执行结果返回给appium server，appium server再把执行结果返回给client。

### 一 安装appium desktop
#### 1.准备工作  
* 安装brew并且更新

        curl -LsSf http://github.com/mxcl/homebrew/tarball/master | sudo tar xvz -C/usr/local --strip 1  
        brew update
    
* 安装node.js
    
        brew install node  

* 安装jdk，此处省略该步骤  
* 安装Android studio  

#### 2.安装Appium  
* go to https://github.com/appium/appium-desktop/releases/tag/v1.15.1 并且下载合适的版本。这里以下载的是mac版本的appium。 
<img src="基于MAC的Appium Desktop环境搭建/appium下载.png" width="50%" height="50%">  
* 安装下载好的安装包,过程较为简单，不赘述。  
* 启动appium desktop，会看到如下界面：
<img src="基于MAC的Appium Desktop环境搭建/appium界面.png" width="50%" height="50%">  
点击启动server后会出现server界面：  
<img src="基于MAC的Appium Desktop环境搭建/appium server.png" width="50%" height="50%">  

#### 3.安装Appium-Doctor

    npm install -g appium-doctor

遇到问题：权限不够无法安装。  

<img src="基于MAC的Appium Desktop环境搭建/appium_doctor安装问题.png" width="50%" height="50%">   
 
解决办法如下：使用管理员权限来运行该命令。

    sudo npm install -g appium-doctor

#### 4.运行Appium-Doctor
打开terminal，运行appium-doctor  

<img src="基于MAC的Appium Desktop环境搭建/appium_desktop运行结果.png" width="50%" height="50%">  

从运行结果中看到需要解决三个问题： 

1） carthage was not found  
2） ANDOID_HOME is not set  
3） JAVA_HOME is not set  
解决办法为：  
1）安装carthage

    brew install arthage

如果提示权限不够就先运行下列代码，再下载carthage：

    sudo chown -R `whoami`:admin/usr/local/bin
    sudo chown -R `whoami`:admin/usr/local/bin/share

2)配置JAVA_HOME和ANDROID_HOME

    1.export ANDROID_HOME=/Users/jytu/Library/Android/sdk
    2.export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_201.jdk/Contents/Home
    3.export PATH=~/bin:$PATH:/usr/local/bin:$ANDROID_HOME/platform-tools/:$JAVA_HOME/bin
    4.export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

再次运行appium-doctor时会显示变量已经配置好。

#### 5.配置appium
打开appium，点击edit configurations，把之前配置好的JAVA_HOME和ANDROID_HOME输入并保存

<img src="基于MAC的Appium Desktop环境搭建/Appium configurations.png" width="50%" height="50%"> 
<img src="基于MAC的Appium Desktop环境搭建/Appium configurations2.png" width="50%" height="50%"> 

此时appium在MAC上就装好了。