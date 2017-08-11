---
title: Network manager tools - network vs NetworkManager
layout: post 
date: 2017-08-04 13:40
category: blog
author: Peng Zou
excerpt: The solution of network conflicting betweent network and NetworkManager
star: ture
tag: 
- network
- NetworkManager
comments: true
---

> 该博客目的在于讨论Linux系统下解决两个网络管理工具network 和NetworkManager管理冲突的问题。

### network vs NetworkManager 简介

简单说network适合使用于服器上也就是网路设定后固定不变使用，而NetworkManager则适合使用于笔记型电脑上必须常常在有线及无线网路环境切换时使用，并且这二个服务所读取及写入的设定档是不同的。   
+ network ：读取的设定档路径为「/etc/sysconfig/network-scripts/*」下的设定档。 
+ NetworkManager ：读取的设定档路径为「/etc/sysconfig/networking/*」下的设定档。     
 
CentOS6 在预设情况下会启动NetworkManager 服务(包含开机启动)，因为个人习惯传统的network 设定，因此建议将NetworkManager 服务停用后再继续后续设定作业，否则在二个服务都启动的情况下将会造成互相干扰的麻烦状况. 
     
### 关闭NetworkManager服务  

当两个网络管理工具同时存在时，运行`service network restart` 会报“设备状态：3 (断开连接)” 的错误。局域网内部访问没有问题，但无法访问外网。如果要消除这个提示，请关闭NetworkManager服务即可。    
+ `chkconfig NetworkManager off`
+ `service NetworkManager stop`      

停掉NetworkManager 服务后，重启网卡`service network restart`, 就能正常上网了。

### network 的IP配置

Linux 系统下配置静态IP的常用命令    
+ `ifconfig` 查看IP、网卡启动和配置情况
+ `ifup eth0` 启动网卡
+ `vi /etc/sysconfig/network-scripts/ifcfg-eth0` 配置IP、网关等数据    

其中ifcfg-eth0 的内容如下      
![]({{ site.url }}/images/ifcfg-eth0.png)

上面黄色背景色部分为默认配置，红色背景色为后来添加配置。
配置完成后，敲入代码：`service network restart`重启网卡服务。这时候尝试`ping www.baidu.com` 应该就没什么问题了。



### 参考文献

+ [network vs NetworkManager](https://www.douban.com/note/280016748/)
+ [centos6.5 关闭NetworkManager服务](http://www.bkjia.com/Linuxjc/879246.html)
+ [Linux 配置静态IP](http://www.cnblogs.com/SunnyZhu/p/5685124.html)




