---
title: 在VMWare中安装CentOS并配置SSH远程登录
categories:
  - 计算机
tags:
  - VMWare
  - CentOS
  - 远程登录
---



今晚折腾了一晚上，本来以为会很轻松，结果刚装用VMWare上CentOS7的时候，连不上网，连SSH都无法访问。郁闷，我之前安装CentOS6的时候并没有做过多的配置，后面在网上查找了很多资料终于成功了，能联网也能用SSH远程登录。



# 配置网络

## 物理机的配置

1. 首先打开VMWare的 `编辑>虚拟网络编辑器` 

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/centOS01.jpg)

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/centOS02.jpg)

可以看到，NET模式下，网关为`192.168.131.2`，可为虚拟机DHCP动态分配`128-254`的IP。

2. 打开Windows的网络与共享中心，配置`VMnet8`虚拟网卡做如下配置:

- 首先勾上`VMWare Bridge Protocol`
- 配置`IPv4`，使用固定IP，将网关配置为刚才看到的网关

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/centOS03.jpg)

3. 再操作可联网的网卡，本人使用的是无线网络，将之共享给虚拟网卡`VMnet8`

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/centOS04.jpg)



## 虚拟机的配置

1. 配置网络

```bash
cd /etc/sysconfig/network-scripts/
vi ifcfg-ens33 # ifcfg-e**** ，名称每台可能不一样
```

2. 编辑此文件，注意配置`BOOTPROTO=dhcp` `ONBOOT=yes`

```bash
TYPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=12509981-530b-403b-bbc0-493447de2d1f
DEVICE=ens33
ONBOOT=yes
```

3. `service network restart`重启网络连接

4. 输入`ifconfig`可以看到，新分配的IP已经生效

   ![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/centOS05.jpg)



## 测试互通性

使用`ICMP`互`ping`测试物理机与虚拟机是否互通。



# 配置SSH

1. 确保CentOS7安装了`openssh-server`，在终端中输入`yum list installed | grep openssh-server`


2. 如果又没任何输出显示表示没有安装  `openssh-server`，通过输入  `yum install openssh-server`进行安装
3. 进入目录`/etc/ssh/`，打开sshd服务配置文件`sshd_config`进行编辑

```bash
cd /etc/ssh
vi sshd_config
```

修改以下内容:

```bash
Port 22 # 打开SSH端口
#AddressFamily any
ListenAddress 0.0.0.0
ListenAddress ::

# 开启允许远程登录
#LoginGraceTime 2m
PermitRootLogin yes

# 使用用户名和密码登录
#PasswordAuthentication yes
#PermitEmptyPasswords no
PasswordAuthentication yes
```

4. 开启  sshd  服务，输入 `service sshd start`
5. 检查  sshd  服务是否已经开启，输入`ps -e | grep sshd`
6. 输入`netstat -an | grep 22`  检查  **22** 号端口是否开启监听



所有配置完毕，此时可以通过远程登录软件比如`putty`、`secureCRT`或者`xshell`进行连接。




# 参考资料

[VMware12中CentOS7网络设置](http://www.linuxidc.com/Linux/2016-08/133998.htm) 

 [虚拟机下CentOS7开启SSH连接](http://blog.csdn.net/tuntun1120/article/details/65443757) 