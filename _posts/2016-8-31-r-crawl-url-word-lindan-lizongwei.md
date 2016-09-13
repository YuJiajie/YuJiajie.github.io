---
layout:post
keywords: rvest R wordcloud2 Rwordseg
description: blog
title: R爬取百度新闻数据
categories: [R随笔]
tags: [R 爬数据 词云 林丹 李宗伟]
group: 
archive
icon: bullhorn
---
据里约奥运闭幕都已过去9天，8月19日那场“李林之战”的余热早已被建林的“小目标”淹没。当时答应的稿子也拖到不了了之（默哀三分钟……
现在只希望将曾经的尝试记录下来，以求不废在总是拾人牙慧却毫无输出面前。在刚应下来写“李林之战”的稿子时的想法很简单：1.搜寻林李对战的历史新闻来源；2.爬取这些新闻的标题或摘要；3.作简单处理后分词做成词云，再简单分析，写个稿子。哪曾知所有的所有都跪倒在“写个稿子”门前（手动黑线，小编都哭了……
1.找林李对战新闻来源
没想到这第一步就是个坑啊！找了一圈竟没发现一家新闻门户有好用的历史新闻入口，最后只能求助新闻搜索，对就是百度新闻。
2.爬取新闻标题+来源和时间+摘要
2016年8月21日，进入百度新闻，搜索[林丹 李宗伟]，共给返回38页结果，其中前37页每页20条，第38页18条，共758条新闻。
{% highlight r %}
library(rvest) #爬取网页数据用到的R包

## 保存第1页网址
url <- "http://news.baidu.com/ns?cl=2&rn=20&tn=news&word=%E6%9E%97%E4%B8%B9%20%E6%9D%8E%E5%AE%97%E4%BC%9F"

## 读取（crawling)网页,UTF-8编码（似乎并没有什么用，编码格式依然保持原样）
## 保存网页中的“标题”，“来源和时间”，“摘要”
Title <- url %>% read_html("UTF-8") %>% html_nodes("h3 a") %>% html_text()
Author <- url %>% read_html("UTF-8") %>% html_nodes("p.c-author") %>% html_text()
Abstract <- url %>% read_html("UTF-8") %>% html_nodes("div.c-summary.c-row") %>% html_text()

## 保存为DataFrame
page <- cbind(Title,Author,Abstract)

## 其余页码的地址用循环生成(共38页,每页20条新闻)
url.other <- sapply(seq(20,37*20,20),function(i) {
  str_c("http://news.baidu.com/ns?word=%E6%9E%97%E4%B8%B9%20%E6%9D%8E%E5%AE%97%E4%BC%9F&pn=",i,"&cl=2&ct=0&tn=news&rn=20&ie=utf-8&bt=0&et=0")
  })

## 每页重复工作
myfun = function(x) {
  Title = url.other[x] %>%read_html("UTF-8") %>% html_nodes("h3 a") %>% html_text()
  Author = url.other[x] %>%read_html("UTF-8") %>% html_nodes("p.c-author") %>% html_text()
  Abstract = url.other[x] %>%read_html("UTF-8") %>% html_nodes("div.c-summary.c-row") %>% html_text()
  cbind(Title,Author,Abstract)
}

## 其他页面提取
page.other = sapply(1:37,function(i) myfun(i))

## 检查爬取失败的情况
evil=as.matrix(cbind(evil=1:37,tf=sapply(1:37,function(i) ncol(page.other[[i]]) !=3)))
evil[evil[,2]==1,]

## 可以看看哪些部分出错
names=matrix(rep(NA,37*6),ncol=6)
for (i in 1:37) {
  names[i,]=colnames(page.other[[i]])
  names[i,duplicated(names[i,1:3])] <- NA
}
cbind(evil[evil[,2]==1,1],names[is.na(names[,3]),1:3])

## 出错的页面重新爬，直到成功为止
i=1
while(i<37){
  if (ncol(page.other[[i]]) != 3) page.other[[i]]=myfun(i)
  i=i+as.numeric(ncol(page.other[[i]])==3)
}

## 再确认一边有没有漏掉的
evil=as.matrix(cbind(evil=1:37,tf=sapply(1:37,function(i) ncol(page.other[[i]]) !=3)))
evil[evil[,2]==1,]

## 合并所页面
i=1
while(i<=37) {
  page=rbind(page,page.other[[i]])
  i=i+1
}
{% endhighlight %}
