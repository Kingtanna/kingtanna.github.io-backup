---
title: Fiddler学习笔记1
date: 2020-02-18 19:40:57
tags: Fiddler
---
Fiddler是一个常用的抓包软件，Fiddler可以拦截请求，在修改参数后重新提交请求。
<!--more-->
1.官网下载[Fiddler](https://www.telerik.com/fiddler)，并进行安装。  
2.由于Fiddler是默认抓取http请求，因此对于https请求，需要更改Fiddler的设置，并在浏览器上安装证书。操作如下：

* **更改Fiddler设置**  
  打开Fiddler，点击Tools>Options>HTTPS，勾选Decrypt HTTPS traffic。 

  <img src="Fiddler学习笔记1/1.png" width="50%" height="50%">
  <img src="Fiddler学习笔记1/2.png" width="50%" height="50%">

* **导出证书**  
  点击Actions按钮，生成证书并导出证书。  

  <img src="Fiddler学习笔记1/3.png" width="50%" height="50%">
  <img src="Fiddler学习笔记1/4.png" width="50%" height="50%">

* 导入证书
  1)这里使用的是chrome浏览器，打开Settings>Privacy and security,点击Privacy and security  

  <img src="Fiddler学习笔记1/5.png" width="50%" height="50%">

  2)点击导入证书，并选取之前生成的证书，继续下一步直到证书导入成功。
 <img src="Fiddler学习笔记1/6.png" width="50%" height="50%">
 <img src="Fiddler学习笔记1/7.png" width="80%" height="80%">
  
3.Fiddler实践  
1)拦截请求，命令：bpu。例如想要拦截 https://www.baidu.com/， 则
在Fiddler命令行中输入
bpu https://www.baidu.com/， 并按下Enter键,命令行下方会提示已设置好断点。    
<img src="Fiddler学习笔记1/8.png" width="50%" height="50%">
&ensp;&ensp;&ensp;<img src="Fiddler学习笔记1/9.png" width="50%" height="50%">  

2)打开chrome浏览器，输入并按下enter键，此时可以发现，网页并不能跳转到百度页面，因为我们设置了对该请求进行拦截，所以此时请求被拦截，网页无法跳转。打开Fiddler，可以看到下图：  

<img src="Fiddler学习笔记1/10.png" width="80%" height="50%">  

如果点击GO,则请求将会释放拦截,网页可以正常跳转。  

   <img src="Fiddler学习笔记1/11.png" width="50%" height="50%">
 
接着搜索Java，网页继续被拦截一直处于loading状态

  <img src="Fiddler学习笔记1/12.png" width="50%" height="50%">

点击找到对应请求，修改参数为C++，并点击run to completation。则搜索结果会变为c++的相关信息。

<img src="Fiddler学习笔记1/13.png" width="50%" height="50%">      
<img src="Fiddler学习笔记1/14.png" width="45%" height="45%">
  

  
