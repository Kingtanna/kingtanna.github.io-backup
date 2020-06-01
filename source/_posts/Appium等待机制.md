---
title: Appium等待机制
date: 2020-02-28 14:05:09
tags: Appium
---
在任何GUI测试代码中，等待机制都是无法避免的一个话题，因为元素的出现都是需要时间的。如何不设置等待时间自动化代码很有可能因为找不到元素而失败，而无限制的加等待的代码又会使得程序运行的时间过长而失去了测试的意义。在appium中也有几种等待机制，需要在程序中合理利用它们。
<!--more-->

### 强制等待

    import time
    time.sleep();  

### 隐式等待（服务端）

    1.python:self.driver.implicitly_wait(5000)
    2.java:driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);

该等待是webdirver提供的一个超时等待。当使用了隐式等待执行测试的时候，如果 WebDriver没有在 DOM中找到元素，将继续等待，超出设定时间后则抛出找不到元素的异常。一旦设置了隐式等待，则它存在整个 WebDriver 对象实例的声明周期中，隐式的等到会让一个正常响应的应用的测试变慢。

### 显示等待（客户端）

    WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)
    driver - WebDriver 的驱动程序(Ie, Firefox, Chrome 或远程)
    timeout - 最长超时时间，默认以秒为单位
    poll_frequency - 休眠时间的间隔（步长）时间，默认为 0.5 秒
    ignored_exceptions - 超时后的异常信息，默认情况下抛 NoSuchElementException 异常。  

WebDriverWait()：同样也是 webdirver 提供的方法。在设置时间内，默认每隔一段时间检测一次当前。页面元素是否存在，如果超过设置时间检测不到则抛出异常。  

WebDriverWait()一般由 until()或 until_not()方法配合使用，下面是 until()和 until_not()方法的说明。
until(method, message=’’)
调用该方法提供的驱动程序作为一个参数，直到返回值不为 False。
until_not(method, message=’’)
调用该方法提供的驱动程序作为一个参数，直到返回值为 False。
