---
layout: post
title: "debian"
date: 2017-04-10
categories: linux
tags: linux
---

* content
{:toc}

由于电脑软件安装多，换了一个系统。以前在虚拟机用过Ubnutu很流畅,这次就换一个Linux其他的版本debian,记录一下遇到的问题。




## 安装系统
首先把iso烧录到u盘中，烧录工具是[老毛桃](http://www.laomaotao.net/)。之后找了个安装debian的教程。  
一路安装中间遇到一个问题，硬件检测时缺2个东西（rt18723befw.bin和rt18168g-3.fw）在网上搜索一番后是有关网络的问题，最后还是没解决。安装时就先跳过网络问题，跟着教程一直安装，大约用了小半天时间装好。  
接下来的工作，首先把用户加入到/etc/sudors以便使用sudo(需root权限)  
```java
(一部分代码)
%sudo   ALL=(ALL:ALL) ALL(原本的)
lwj（你的用户名）     ALL=(ALL:ALL) ALL(需要添加的)
```
这里有个问题用vi不行，按上下左右在编辑器里会变成大写的ABCD(因为系统预安装的版本问题),所以用gedit命令编辑,或者安装vim  
```java
apt-get install vim
```

---


## 安装无线驱动
(有的可能遇到这个问题)最开始安装好时，没有无线标志，对于笔记本来说很难受，没办法先用插网线。  
配置软件源  
```html
deb http://httpredir.debian.org/debian/ jessie main contrib non-free  
```
安装驱动  
```html
apt-get install firmware-*
```
这里会安装许多的东西，很多也不明白有什么用先留着  
安装完后重启计算机  
多次重启有一个问题，开机只能用一会儿无线网就会断，重启又可以用一会，解决办法：把/lib/firmware下的文件全删，留下rtl_nic rtlwifi文件夹(开始装系统所缺少的2个文件)，问题解决。  

---


## 安装输入法
在左上角的活动里可以搜索fcitx配置里面好像自带了Google 拼音，开始不知到有这个去下的搜狗用，自己觉得能打中文就行，其他的没太大关系，这里可以按win+m右边可以选择用那个输入法，也可以设置快捷键。

---
**文章声明：自由转载-非商用-非衍生-保持署名**





