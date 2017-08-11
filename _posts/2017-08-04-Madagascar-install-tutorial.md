---
title: Madagascar install guidline
layout: post
date: 2017-08-04 15:10 
category: blog
author: Peng Zou
excerpt: Madagascar install 
star: ture 
tag: 
- madagascar 
- atlas
comments: true 
--- 

> 该博客旨在讨论Linux系统CentOS 和Ubuntu 下的地球物理专业软件Madagascar的安装。      

[Madagascar](http://www.ahay.org/wiki/Main_Page) 是一个地球物理专家开发的主要用来处理勘探地球物理数据资料或进行模型测试的软件包，它是完全开源（免费）的。Madagascar提供了一个针对多维数据分析程序的集合，包括地震成像、地震数据处理和对有组织格式的数据进行处理流程实现与计算实验的综合环境。Madagascar遵从的是Claerbout（1992）提出的可重复性（再生性）研究的思想原理。

### 源码下载 （三种方法）

1.  从[网站上](http://sourceforge.net/projects/rsf/files) 下载压缩包 madagascar-*.*.tar.gz，然后用tar -xzvf madagascar-*.*.tar.gz解压。
2. 用svn 命令下载  
`svn co https://rsf.svn.sourceforge.net/svnroot/rsf/trunk RSFSRC`   
3. 用git 命令下载 
`git clone https://github.com/ahay/src RSFSRC`    

本人推荐直接用命令下载，因为用命令会直接把最新的源码下载到RSFSRC目录。另外跟新方便，直接用命令`svn update` 和`git pull` 即可。 

### 安装路径设置  
在自己的home下面建两个文件夹，RSF, RSFTMP。
+ `mkdir RSF`  安装路径
+ `mkdir RSFTMP`   数据文件的存放路径

### 安装前检查依赖库   

`./configure --prefix=/完整的安装路径 API=c, c++, matlab, gfortran`    
API后面是接口，默认接口是c, c++。此步会自动检测依赖库的安转情况，请自行安转自己需要的依赖库，然后再执行上面命令。

#### 依赖库安装 
+ Ubuntu 系统直接用 `sudo apt-get install` + package 即可, 可参照[这里](http://blog.sciencenet.cn/home.php?mod=space&uid=898810&do=blog&id=674969)的依赖库。
+ CentOS 系统可用自带的软件管理工具安转。或者`yum search`+key 搜索到软件包，然后`yum install`+package 安装即可。
+ CentOS 7 需要用atlas 包来代替cblas 和lapack库，并对 framework/configure.py文件做如下修改。![]({{ site.url }}/images/madagascar_install.png)
具体安转错误排除可在[这里](http://www.ahay.org/wikilocal/docs/3_Madagascarschool-Qingdao-Wang.pdf)查看。 
+ Python 的有关依赖库建议安转[anaconda](https://www.continuum.io/downloads/)。


### 编译和安装 

依赖库安装完毕后，没有自己需要的命令提示不能安转，就用下面的两个命令编译安装即可。  
+ `make` or `scons`
+ `make install` or `scons install` 

### 最后的配置 
全部编译安装完成后，打开 $RSFROOT/etc/madagascar/env.sh 文件， 把里面的DATAPATH变量改成~/RSFTMP。
并把madagascar的环境变量设置好。
打开 ~/.bashrc文件，将`source $RSFROOT/etc/madagascar/env.sh` 添加到文件的末尾，注意，RSFROOT要
写成前面的完整路径最后，保存，退出，在终端中输入 `source .bashrc`。madagascar的安装就完成了。
在终端输入`sfin`命令，如能看到该命令的用法提示，说明安装成功。


