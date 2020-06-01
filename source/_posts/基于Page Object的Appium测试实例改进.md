---
title: 基于Page Object的Appium测试实例改进
date: 2020-03-27 19:37:26
tags: Appium
---
前段时间我在极客时间上订阅了Mobile测试的相关课程，在学习了一些基础知识后，跟着老师以及网上的教程搭建了基于MAC的appium运行环境，并在此基础上使用appium的record功能生成了测试Android app的第一个例子。由于实际中record功能使用的并不多，因此我跟着视频进行了下一步的测试改进。
<!--more-->
首先下载Android Studio和IDEA，加上appium，这三者的作用分别为：  
- appium -- server   
- IDEA -- client  
- Android Studio -- 手机模拟器   

打开IDEA将之前record生成的代码拷贝进去，并添加相应的依赖和包，运行后测试用例通过。该测试用例完成的动作是打开雪球app，关闭弹窗后点击搜索框并输入alibaba，代码如下：  

    import io.appium.java_client.MobileElement;
    import io.appium.java_client.android.AndroidDriver;
    import junit.framework.TestCase;
    import org.junit.After;
    import org.junit.Before;
    import org.junit.Test;
    import java.net.MalformedURLException;
    import java.net.URL;
    import org.openqa.selenium.remote.DesiredCapabilities;

    public class SampleTest {

    private AndroidDriver driver;

    @Before
    public void setUp() throws MalformedURLException {
        DesiredCapabilities desiredCapabilities = new DesiredCapabilities();
        desiredCapabilities.setCapability("platformName", "android");
        desiredCapabilities.setCapability("deviceName", "test");
        desiredCapabilities.setCapability("appPackage", "com.xueqiu.android");
        desiredCapabilities.setCapability("appActivity", ".view.WelcomeActivityAlias");
        desiredCapabilities.setCapability("autoGrantPermissions", "true");

        URL remoteUrl = new URL("http://localhost:4723/wd/hub");

        driver = new AndroidDriver(remoteUrl, desiredCapabilities);
    }

    @Test
    public void sampleTest() {
        MobileElement el1 = (MobileElement) driver.findElementById("com.xueqiu.android:id/tv_search");
        el1.click();
        MobileElement el2 = (MobileElement) driver.findElementById("com.xueqiu.android:id/search_input_text");
        el2.sendKeys("alibaba");
    }

    @After
    public void tearDown() {
        driver.quit();
    }
    }

这是一个非常简单的例子，从代码中可以看到，无论是测试开始前driver的初始化还是页面元素的查找和操作以及最后driver的关闭都集中写在一个文件中。如果增加多个这样的文件，那么每一个文件都要增加driver初始化和关闭的代码，并且每一次页面的元素发生变化那么用到该元素的文件均需要做出改变。这种代码是极其脆弱且难以维护的。那么有没有什么方法可以解决上述问题呢？  

答案是有！早在2013年，软件大师Martin Fowler就写下了Page Object的文章，他认为应该用Page Object的模式来构建自动化测试代码。Page Object的一个基本经验法则是凡是人类能做的事page object通过软件客户端都能够做到。它也应当提供一个易于编程的接口并隐藏窗口中底层的部件。也就是说可以使用Page Object来封装一个html页面或部分页面，测试人员通过Page Object提供的应用程序特定的API接口来操作页面元素，而不需要在在原来的HTML页面中直接寻找你、和操作元素。这样做的好处是：
- 减少代码的重复  
- 提高测试用例的可读性  
- 提高测试用例的可维护性  

下面这张图描述了一般基于Page Object模式测试代码各个模块的作用

<img src="Appium学习笔记3-基于Page Object的测试实例改进/POM.png" width="60%" height="60%"> 

从上图可以知道，基于Page Object model的测试用例组织结构中，每一部分都有其专门的作用。例如page用来对页面进行封装，而不用来进行其他的操作；driver只用来驱动各种接口；testcase只用来写测试用例。下面我们按照该结构来更改这个简单的测试用例。  

#### --第0步--：  
driver初始化动作以及测试case都写在一个文件上：demo.java   

#### --第1步--：  
<img src="Appium学习笔记3-基于Page Object的测试实例改进/first_step.png" width="80%" height="80%">  

创建BasePage来管理driver，把driver的代码挪到BasePage.java，在该class文件中构造setup()函数，返回driver。在demo.java中增加原函数的构造函数，利用构造函数初始化driver。此时只把driver的初始化代码挪出去，而到首页以及点击搜索和搜索阿里巴巴的代码仍然留在demo.java中。  

#### --第2步--：  
<img src="Appium学习笔记3-基于Page Object的测试实例改进/second_step.png" width="80%" height="80%">  

创建BaseMainPage.java，将点击搜索从而到达search页面的代码挪到BaseMainPage.java中。创建BaseSearchPage.java，将点击搜索框和输入阿里巴巴的代码挪到BaseSearchPage.java中。此时demo.java中只剩下初始化BasePage，BaseMainPage，BaseSearchPage的代码，其中demo.java的构造函数拿到BasePage初始化的driver，BaseMainPage，BaseSearchPage分别拿到driver后调用GoToSearchPage(driver)和Search(driver,”alibaba”)。这个时候的代码虽然已有page object 的模式，但是可以看到，driver还是暴露在demo.java文件中，这样是不合理的。因为page object模式的初衷就是为了隐藏底层代码。

#### --第3步--：
<img src="Appium学习笔记3-基于Page Object的测试实例改进/third_step.png" width="80%" height="80%">  

改变BasePage.java的返回值，使其不再返回driver而是返回进入app后的主页面，这一步骤相当于是返回了带有初始化的driver的主页面。因此不需要再在demo.java中再次初始化BaseMainPage，给其初始化的driver。以此类推，在BaseMainPage.java中的GoToSearchPage方法也可以返回初始化的driver的search page。这样以后在demo.java页面中只需要初始化BasePage,并且调用相应的函数就能够一次得到初始化的Main page和search page

#### --第4步--：
<img src="Appium学习笔记3-基于Page Object的测试实例改进/fourth_step.png" width="80%" height="80%">  

把关闭server的代码也放到BasePage里面

#### --第5步--：
<img src="Appium学习笔记3-基于Page Object的测试实例改进/fifth_step.png" width="60%" height="60%">  

新建BaseBasePage，并创建driver， 使BaseMainPage和BaseSearchPage继承该page，从而这两个page不需要再次创建自己的driver。封装FindElementById方法，将其写入BaseBasePage中，从而继承它的类都可以直接调用该方法。

经过5步后就形成了基本的page object模式。但这还只是一个简单的例子，不涉及到data以及其他的一些配置文件，这些需要根据项目的需要进一步改进。在使用Page Object时候有以下需要注意的地方：
- public方法代表Page提供的功能
- 尽量不要暴露Page的内部细节
- 不要assertion
- 方法可以返回其他Page Objects
- Page Objects不用代表整个页面，可以是任意一个部分
- 一样的操作，不同的结果应该分开（正确登录，错误登录）