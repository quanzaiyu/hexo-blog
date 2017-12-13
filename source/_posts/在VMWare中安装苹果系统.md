---
title: 在VMWare中安装苹果系统
categories:
  - 计算机
tags:
  - VMWare
---

# 前言

VMWare本身并不支持OSX的安装，但是可以通过破解补丁`unlocker208`可以突破限制，安装苹果系统。

之所以选择使用虚拟机安装`OSX`，主要是由于本人太穷，买不起苹果机，个人只是出于兴趣想要研究下苹果系统，另外就是想学一下`Swift`，而用一下`XCode`。



## 各种工具的下载地址

下载VMware 12 Pro

- http://pan.baidu.com/s/1hrSuPZe 提取密码：7qz3

下载VMware虚拟机mac Os补丁(Unlocker补丁工具)

- http://pan.baidu.com/s/1pLLBnf1 提取密码：pcuu

下载OS X Mavericks Install.cdr（苹果系统）

- http://pan.baidu.com/s/1jI78s4Y  提取密码：drbh



# 安装流程

## 安装VMWare虚拟机

不多说了，很简单，网上教程很多。



## 使用unlocker解锁OSX

安装unlocker以解锁OSX



## 安装OSX系统

首先使用VMWare创建一个虚拟机，分配磁盘至少60GB，创建完成后，修改对应虚拟机目录下的`xxx.vmx`文件，在`smc.present = "TRUE"`后添加:

```
smc.version = 0
```

之后开启虚拟机，等待进度条跑完。

当选择安装磁盘的时候，会出现`OSX Base System 空间不够`的错误提示，此时得对硬盘重新分区:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/OSX01.jpg)

重新分区完毕，选择安装磁盘为新划分出来的磁盘即可

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/OSX02.jpg)

之后就是安装过程

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/OSX03.jpg)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/OSX04.jpg)

安装完成后的界面:

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/OSX05.jpg)

这下子就可以体验OSX系统的各种操作了，最主要的是可以安装XCode进行苹果软件开发了!




# 参考资料

[IT之家学院：一步一步教你VMWare安装苹果Mac OS X](https://www.ithome.com/html/mac/312273.htm) 

[50CTO: 苹果系统初体验 之 虚拟机安装MAC OS X Mavericks](http://blog.51cto.com/zhaoyuqiang/1430686) 