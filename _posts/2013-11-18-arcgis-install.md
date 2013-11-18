---
layout: post
keywords: arcgis
description: blog
title: "Arcgis10.0 安装+破解"
categories: [Blog]
tags: [教程]
group: archive
icon: bullhorn
---

*（此教程本就是留作自用的，直接从[原博客](http://blog.sina.com.cn/s/blog_9caf98ff010163ja.html)复制，时间上的也不再修改）*

本人非常小白，以下都是从网上看来的，自己用了，有时行有时不行（可能是自己某些地方弄错了），安了几台之后终于好一点了，总结一下容易错的地方，留作以后查看。

## 下载GIS软件包和破解文件

Esri官网上10.1已经可以试用了，但是要破解估计还有一段时间，官网也挂出了sp4的补丁，[这里](http://www.filestube.com/shareflare/2FUpI54Go2NFPr5N0XzsC3/esri-arcgis-10-sp4-x86x64.html)也能下。本人是从9.3转到10.0的，开始下过中文的（起初以为会方便，装了后看着不怎么习惯），后来换回英文（有问题容易搜索），我自己安的可能是sp1。

## 下载破解包

可以到[这里](https://www.dropbox.com/s/ksaclajforg3975/ArcGIS10.0%E5%AE%8C%E7%BE%8E%E7%A0%B4%E8%A7%A3.zip)下载。怎么工作的本人不懂，总之就是 覆盖5个文件（另外有一个是最后复制进去bin里），导入两个注册表。

## 安装license manager

这个在GIS的安装文件里就有，有人提议先装这个再装主程序会比较顺利。装完后停掉许可服务。

## 安装主程序

这个只需点击安装文件里的项就行，最后可以把一些有需要的也一并安上。

![setup-gis][setup-gis]

## 破解

1.复制5个文件到bin文件夹，license manager默认路径安装的就是（C:\Program
Files\ArcGIS\License10.0\bin）;关键是9.xlic.lic那个文件（也可以是其它名称如service.lic），可以自己用注册机制作，做的时候注意选择正确的feature和version；修改文件里的this-host为你的计算机名（**不能是中文**）；

![bin-gis][bin-gis]

2.打开bin文件夹里的lmtools.exe进行设置，只要选择license file就行，就是bin里的那个.lic文件(注册机做的)；

![lmtools-gis][lmtools-gis]

3.开启服务，不行就停掉再开，多试几次就可以了；

![lmtools2-gis][lmtools2-gis]

4.检查license，出现最后一行就行了，如果不行，自己再做一个.lic；

![lmtools3-gis][lmtools3-gis]

5.打开ArcGIS administrator设置（确保license manager已关）；

![administrator1-gis][administrator1-gis]

6.设置下面几个，许可管理器记得改为localhost；

![administrator2-gis][administrator2-gis]

7.刷新一下就能看到许可了；

![administrator3-gis][administrator3-gis]

8.复制AfCore.dll文件进bin文件夹；导入两个注册表文件

![regist-gis][regist-gis]

OK！试用，好运……
	

[setup-gis]: /image/post/setup-gis.jpg
[bin-gis]: /image/post/bin-gis.png
[administrator1-gis]: /image/post/administrator1-gis.png
[administrator2-gis]: /image/post/administrator2-gis.png
[administrator3-gis]: /image/post/administrator3-gis.png
[lmtools-gis]: /image/post/lmtools-gis.png
[lmtools2-gis]: /image/post/lmtools2-gis.png
[lmtools3-gis]: /image/post/lmtools3-gis.png
[regist-gis]: /image/post/regist-gis.png
