---
layout: post
keywords: 
description: blog
title: 栅格计算器叠加不同范围的栅格
categories: [笔记]
tags: [ArcGis 栅格计算器]
group: archive
icon: bullhorn
---

最近用ArcGis 10帮同学做不同栅格的叠加计算，遇到些问题，google解决后记录于此。目的很简单，就是把不同栅格进行叠加，重叠的区域进行加和，不重叠区域保持不变。步骤如下：

1. 打开Spatial Analyst Tools > Map Algebra > Raster Caculator

2. Environment Settings > Processing Extent > Extent 设置为将要计算的栅格中extent最大的栅格

3. 用Con()和IsNull()函数进行计算，如下：
{% highlight python %}
Con( IsNull( "raster1" ) + IsNull( "raster2" ) <> 2, ( Con( IsNull("raster1" ) , 0 , "raster1" ) + Con( IsNull( "raster2" ) , 0 , "raster2")))
{% endhighlight %}

函数将栅格分两部分处理'<>2,'之前的处理两个栅格都为空值的区域：“两个栅格都为空即为空”；之后的处理只要有一个栅格不为空的区域：为两部分的加和，Con()目的为把两个栅格不为空的区域转为extent一致的栅格，即将栅格为空的区域用0代替，其余为栅格原始值。

需注意的是函数的大小写和运算符之间的空格。

参考Esri中国社区[来生缘](http://bbs.esrichina-bj.cn/ESRI/viewthread.php?tid=54711&extra=&page=1)对相应问题的回答。


