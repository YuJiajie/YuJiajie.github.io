---
layout: post
keywords: Crested ibis sibling rivalry
description: blog
title: 朱鹮幼鸟竞争
categories: [Blog]
tags: [科研笔记]
group: archive
icon: bullhorn
---
{% include pengx/setup %}

2013年5月12日~6月12日，我们对研究区域内4对朱鹮的巢进行了观察，并利用便携式数码摄像机对其中一个2只幼体的巢进行了详细拍摄。拍摄点选在距营巢树约20m处的山坡上，视野开阔并有适量遮蔽物。野外全天工作时间为6:00~18:00，从观察到亲鸟飞回巢区时开始录制，直到亲鸟飞离巢区后或停于周边树枝休息幼鸟无明显争食行为后结束录制。同时，研究人员用10倍的双筒望远镜进行观察，记录亲鸟回巢时间、进食个体及次序、幼鸟活动情况等基本信息以补充校正视频信息。感谢李欣海老师、翟天庆局长、王远师姐以及杜峰村的村民们。

# 数据

{% highlight r %}
Feed <- read.csv("D:/zhuhuan/CrestedIbisFeedingData_check_combination.csv", 
    header = T, stringsAsFactors = F)
str(Feed)
{% endhighlight %}



{% highlight text %}
## 'data.frame':	957 obs. of  22 variables:
##  $ ID            : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Date          : chr  "2013-5-18" "2013-5-18" "2013-5-18" "2013-5-18" ...
##  $ Weather       : chr  "多云" "多云" "多云" "多云" ...
##  $ Time_B        : chr  "6:12:20" "6:12:58" "6:13:18" "6:13:36" ...
##  $ Time_I        : int  38 20 18 76 157 55 23 18 30 28 ...
##  $ Video_N       : chr  "MOV01B" "MOV01B" "MOV01B" "MOV01B" ...
##  $ Bout          : int  1 1 1 1 1 2 2 2 2 2 ...
##  $ Section       : int  1 2 3 4 5 1 2 3 4 5 ...
##  $ Attack_F      : chr  "B" "B" "N" "B" ...
##  $ Attack_A      : int  7 0 0 0 0 4 3 0 0 0 ...
##  $ Attack_ABA    : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Attack_B      : int  6 3 0 12 32 3 0 0 4 0 ...
##  $ Attack_ABB    : int  0 0 0 0 16 0 0 0 0 0 ...
##  $ Attack        : chr  "M" "B" "N" "B" ...
##  $ Begging       : chr  "M" "M" "M" "M" ...
##  $ Feed_ID       : int  1 2 3 4 0 1 2 3 4 5 ...
##  $ Feed          : chr  "A" "A" "A" "A" ...
##  $ Abdicate      : chr  "O" "A" "O" "A" ...
##  $ Attack_Freq_A : num  0.367 0 0 0 0 ...
##  $ Attack_Freq_B : num  0.322 0.15 0 0.158 0.204 ...
##  $ Attack_total_A: int  7 0 0 0 0 4 3 0 0 0 ...
##  $ Attack_total_B: int  6 3 0 12 48 3 0 0 4 0 ...
{% endhighlight %}



{% highlight r %}
# 另存为Feed1
Feed1 <- Feed
# 去除没有进食的回合
Feed1 <- Feed1[!Feed1$Feed_ID == 0, ]
table(Feed1$Feed_ID)
{% endhighlight %}



{% highlight text %}
## 
##   1   2   3   4   5   6   7   8   9  10  11 
## 173 173 160 113  71  46  27  13   6   2   1
{% endhighlight %}


# 基本信息

包括不同个体啄击、进食的情况以及亲鸟喂食的基本信息

## 攻击情况

B个体的各类啄击行为都显著高于A个体


{% highlight r %}
# 总攻击次数
(at <- lapply(Feed[, 21:22], function(x) sum(x, na.rm = T)))
{% endhighlight %}



{% highlight text %}
## $Attack_total_A
## [1] 4199
## 
## $Attack_total_B
## [1] 5363
{% endhighlight %}



{% highlight r %}
chisq.test(unlist(at))
{% endhighlight %}



{% highlight text %}
## 
## 	Chi-squared test for given probabilities
## 
## data:  unlist(at)
## X-squared = 141.7, df = 1, p-value < 2.2e-16
{% endhighlight %}



{% highlight r %}
# 真实攻击次数
(att <- lapply(Feed[, c("Attack_A", "Attack_B")], function(x) sum(x, na.rm = T)))
{% endhighlight %}



{% highlight text %}
## $Attack_A
## [1] 2803
## 
## $Attack_B
## [1] 3518
{% endhighlight %}



{% highlight r %}
chisq.test(unlist(att))
{% endhighlight %}



{% highlight text %}
## 
## 	Chi-squared test for given probabilities
## 
## data:  unlist(att)
## X-squared = 80.88, df = 1, p-value < 2.2e-16
{% endhighlight %}



{% highlight r %}
# 伪攻击次数
(atf <- lapply(Feed[, c("Attack_ABA", "Attack_ABB")], function(x) sum(x, na.rm = T)))
{% endhighlight %}



{% highlight text %}
## $Attack_ABA
## [1] 1396
## 
## $Attack_ABB
## [1] 1845
{% endhighlight %}



{% highlight r %}
chisq.test(unlist(atf))
{% endhighlight %}



{% highlight text %}
## 
## 	Chi-squared test for given probabilities
## 
## data:  unlist(atf)
## X-squared = 62.2, df = 1, p-value = 3.098e-15
{% endhighlight %}



{% highlight r %}
# 无效攻击次数
(atu <- lapply(Feed[Feed$Feed_ID == 0, c("Attack_total_A", "Attack_total_B")], 
    function(x) sum(x, na.rm = T)))
{% endhighlight %}



{% highlight text %}
## $Attack_total_A
## [1] 774
## 
## $Attack_total_B
## [1] 891
{% endhighlight %}



{% highlight r %}
chisq.test(unlist(atu))
{% endhighlight %}



{% highlight text %}
## 
## 	Chi-squared test for given probabilities
## 
## data:  unlist(atu)
## X-squared = 8.222, df = 1, p-value = 0.004139
{% endhighlight %}


## 递食情况

总共拍摄记录17天，每天平均回巢喂食10次、递食46次，每次回巢平均递食5次


{% highlight r %}
# 总共录像天数
length(unique(Feed1$Date))
{% endhighlight %}



{% highlight text %}
## [1] 17
{% endhighlight %}



{% highlight r %}
# 有喂食回巢次数
length(unique(Feed1$Bout))
{% endhighlight %}



{% highlight text %}
## [1] 173
{% endhighlight %}



{% highlight r %}
# 每天有喂食回巢的平均次数
length(unique(Feed1$Bout))/length(unique(Feed1$Date))
{% endhighlight %}



{% highlight text %}
## [1] 10.18
{% endhighlight %}



{% highlight r %}
# 总共递食次数
length(Feed1$Feed)
{% endhighlight %}



{% highlight text %}
## [1] 785
{% endhighlight %}



{% highlight r %}
# 每天递食的平均次数
length(Feed1$Feed)/length(unique(Feed1$Date))
{% endhighlight %}



{% highlight text %}
## [1] 46.18
{% endhighlight %}



{% highlight r %}
# 单次有喂食回巢中递食的平均次数
source("D:/R/function/Diff_vector.R")
Frequence <- rev(Diff.vector(rev(as.numeric(table(Feed1$Feed_ID)))))
Times <- 1:11
lapply(list(mean, sd), function(f) f(rep(Times, Frequence)))
{% endhighlight %}



{% highlight text %}
## [[1]]
## [1] 4.538
## 
## [[2]]
## [1] 1.825
{% endhighlight %}


## 不同个体进食分配

不同个体进食的次数上无显著差异


{% highlight r %}
(taf <- table(Feed1[, 17]))
{% endhighlight %}



{% highlight text %}
## 
##   A   B 
## 413 367
{% endhighlight %}



{% highlight r %}
abp <- barplot(table(Feed1[, 17]), xlab = "Individual", ylab = "Feeding Times", 
    main = "Feed Distribustion", ylim = c(0, 450), xpd = FALSE)
text(abp, taf + 10, format(taf), col = "blue")
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-4.png) 

##### 图1 不同个体进食分配

### 差异性检验

{% highlight r %}
chisq.test(taf)  # A and B 
{% endhighlight %}



{% highlight text %}
## 
## 	Chi-squared test for given probabilities
## 
## data:  taf
## X-squared = 2.713, df = 1, p-value = 0.09954
{% endhighlight %}


# 进食和攻击的周期性变化

进食比例、啄击次数的周期变化情况以及不同个体乞得进食所需时间的动态情况

## 进食比例的周期变化

竞争回合对应的是单次回巢喂食中递食的次序，由于单次回巢平均递食5次，所以越靠后的竞争回合出现的递食次数越少。进食比例动态基本成以2次进食为周期的波动：先是A个体主要进食，两回合后换为B个体主要进食。


{% highlight r %}
library(ggplot2)
# theme_set(theme_bw()) # 设置为白底主题
Prop <- read.csv("D:/zhuhuan/Prop.csv", header=T, stringsAsFactors=F)
Prop$Sequence <- as.factor(Prop$Sequence)
Prop$Sequence <- factor(Prop$Sequence,levels=levels(Prop$Sequence))

qplot(x=Sequence,y=Proportion,data=Prop,group=Group)+
  geom_line(aes(linetype=Group))+scale_linetype_manual(name="",labels=c(" A个体"," B个体"),values=c(1,5))+
  geom_point() + xlab('竞争回合')+ylab("进食比例")+
  theme(axis.text=element_text(size=15,colour=I('black')),
        axis.title=element_text(size=20,colour=I('black')),
        legend.text=element_text(size=15),legend.position=c(0.5,0.9),legend.key=element_blank())
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-6.png) 

##### 图2 不同个体进食比例的周期性变化（平均数±标准误）

## 啄击次数的周期变化

啄击次数的动态与进食比例的动态吻合良好，B个体在靠后的竞争回合中啄击次数更多，A个体恰好相反。


{% highlight r %}
FeedBoutError <- read.csv("D:/zhuhuan/FeedBoutError.csv", header=T, stringsAsFactors=F)
FeedBoutError$Feed_ID <- as.factor(FeedBoutError$Feed_ID)
FeedBoutError$Feed_ID <- factor(FeedBoutError$Feed_ID, levels=levels(FeedBoutError$Feed_ID))

Error <- ggplot(data = FeedBoutError,mapping = aes(x=Feed_ID,group=Individual))
Error + aes(y=Attack_Mean, ymin=Attack_lower, ymax=Attack_upper)+ 
  geom_errorbar(aes(linetype=Individual),width=.4) + geom_line(aes(linetype=Individual))+
  scale_linetype_manual(name="",labels=c(" A个体"," B个体"),values=c(5,1)) + geom_point() + 
  xlab('竞争回合') + ylab('啄击次数')+
  theme(axis.text=element_text(size=15,colour=I('black')),
        axis.title=element_text(size=20,colour=I('black')),
        legend.text=element_text(size=15),legend.position=c(0.5,0.9),
        legend.key=element_blank())
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-7.png) 

##### 图3 不同个体啄击次数的周期性变化（平均数±标准误）

## 进食的时间消耗的动态

反映不同个体在各个竞争回合中平均每次获得食物所需的时间（平均数±标准误），B个体略微高于A个体。


{% highlight r %}
Error+ aes(y=Time_Mean, ymin=Time_lower, ymax=Time_upper) + geom_errorbar(aes(linetype=Individual),width=.4) +
  geom_line(aes(linetype=Individual))+scale_linetype_manual(name="",labels=c(" A个体"," B个体"),values=c(5,1)) + geom_point() + 
  xlab('竞争回合') + ylab('获得进食需要的时间（秒/次）')+
  theme(axis.text=element_text(size=15,colour=I('black')),
        axis.title=element_text(size=20,colour=I('black')),
        legend.text=element_text(size=15),legend.position=c(0.5,0.9),
        legend.key=element_blank())
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-8.png) 

##### 图4 不同个体获得食物所需的时间（平均数±标准误）

## 不同个体在取食上花费的时间无显著性差异

给出散点分布信息并做对应分布的差异性检验


{% highlight r %}
# A、B取得进食花费的时间
Feed2 <- Feed1[!Feed1$Feed=='N', ]
Feed2 <- Feed2[!is.na(Feed2$Time_I),]
Time_A <- Feed2[Feed2$Feed=="A","Time_I"]
Time_B <- Feed2[Feed2$Feed=="B","Time_I"]
x_a=(Feed2[Feed2$Feed=="A","Feed_ID"])
x_b=(Feed2[Feed2$Feed=="B","Feed_ID"])
# 绘图
plot(Time_A~x_a, xlab="竞争回合", ylab="获得进食需要的时间（秒/次）",ylim=c(-3.760,448),pch=4,col="blue")
points(Time_B~x_b,pch=3,col='red',cex=.7)
lines(FeedBoutError[FeedBoutError$Individual=='A','Time_Mean']~seq(1,11,1),col="blue")
points(FeedBoutError[FeedBoutError$Individual=='A','Time_Mean']~seq(1,11,1),col="blue",pch=20)
lines(FeedBoutError[FeedBoutError$Individual=='B','Time_Mean']~seq(1,11,1),col="red")
points(FeedBoutError[FeedBoutError$Individual=='B','Time_Mean']~seq(1,11,1),col="red",pch=20)
legend(9,430,legend=c('A个体','B个体'),pch=c(4,3),col=c('blue','red'))
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-9.png) 

##### 图5 不同个体获得食物所需的时间

### 原始数据分布不正态

{% highlight r %}
par(mfrow = c(1, 2))
hist(Time_A)
hist(Time_B)
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-10.png) 


### 做对数变换后

{% highlight r %}
par(mfrow = c(1, 2))
hist(log(Time_A))
hist(log(Time_B))
par(mfrow = c(1, 1))
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-11.png) 


### 对变换后数据做t检验

{% highlight r %}
t.test(log(Time_A), log(Time_B))
{% endhighlight %}



{% highlight text %}
## 
## 	Welch Two Sample t-test
## 
## data:  log(Time_A) and log(Time_B)
## t = -1.544, df = 690.7, p-value = 0.1231
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -0.18519  0.02216
## sample estimates:
## mean of x mean of y 
##     4.042     4.123
{% endhighlight %}


### 对原始数据做非参数检验(‘Mann-Whitney’检验)

{% highlight r %}
wilcox.test(Time_A, Time_B)
{% endhighlight %}



{% highlight text %}
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  Time_A and Time_B
## W = 58419, p-value = 0.07265
## alternative hypothesis: true location shift is not equal to 0
{% endhighlight %}


# 攻击与进食的相关性分析

由于递食11次的情况只出现过1次，在算进食比例时会有很大的误导，所以将其去除。去除后，对于A个体啄击次数与进食的比例和进食次数都有显著的正相关；对于B个体，啄击次数与进食比例显著正相关，与进食次数不相关。

## A个体啄击次数与进食比例的线性相关情况

{% highlight r %}
Attack_Mean_A <- FeedBoutError[FeedBoutError$Individual=="A","Attack_Mean"]
Proportion_A <- Prop[Prop$Group=="A","Proportion"]
cor_P_A1 <- cor.test(Attack_Mean_A,Proportion_A)
cor_P_A2 <- cor.test(Attack_Mean_A[-11],Proportion_A[-11])

plot(Attack_Mean_A,Proportion_A,xlab="特定回合中A个体平均啄击次数",ylab="特定回合中A个体进食比例",cex.lab=1.5)
abline(lm(Proportion_A~Attack_Mean_A))
text(3,0.9,labels=paste0("r = ",round(cor_P_A1$estimate,3),"\n",
                            "p = ",round(cor_P_A1$p.value,3)))

# 去除离群值11（整个育幼期亲鸟单次回巢喂食11次的情况只出现过一次）
points(Attack_Mean_A[11], Proportion_A[11], col = "red")
text(2.2,1,"11",col="red")
abline(lm(Proportion_A[-11]~Attack_Mean_A[-11]), col="red")
text(5,0.9,labels=paste0("r = ",round(cor_P_A2$estimate,3),"\n",
                            "p = ",round(cor_P_A2$p.value,3)),col="red")
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-14.png) 

#####图6 A个体体啄击次数与进食比例的相关性

## A个体啄击次数与进食次数的线性相关情况

{% highlight r %}
Num.feed_A <- Prop[Prop$Group=="A","Ng"]
cor_Num_A1 <- cor.test(Attack_Mean_A,Num.feed_A) 
cor_Num_A2 <- cor.test(Attack_Mean_A[-11],Num.feed_A[-11]) 

plot(Attack_Mean_A,Num.feed_A,xlab="特定回合中A个体平均啄击次数",ylab="特定回合中A个体平均进食次数",cex.lab=1.5)
abline(lm(Num.feed_A~Attack_Mean_A))
text(3,120,labels=paste0("r = ",round(cor_Num_A1$estimate,3),"\n",
                            "p = ",round(cor_Num_A1$p.value,3)))

# 去除离群值11（整个育幼期亲鸟单次回巢喂食11次的情况只出现过一次）
points(Attack_Mean_A[11], Num.feed_A[11], col = "red")
text(2.2,1,"11",col="red")
abline(lm(Num.feed_A[-11]~Attack_Mean_A[-11]), col="red")
text(5,120,labels=paste0("r = ",round(cor_Num_A2$estimate,3),"\n",
                            "p = ",round(cor_Num_A2$p.value,3)),col="red")
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-15.png) 

##### 图7 A个体体啄击次数与进食次数的相关性

## B个体啄击次数与进食比例的线性相关情况

去除11回合后显著性降低，但p值依然小于0.05，因为对于B来说第9，10回合中的攻击性格外强，对相关性影响大于11回合。


{% highlight r %}
Attack_Mean_B <- FeedBoutError[FeedBoutError$Individual=="B","Attack_Mean"]
Proportion_B <- Prop[Prop$Group=="B","Proportion"]
cor_P_B1 <- cor.test(Attack_Mean_B,Proportion_B)
cor_P_B2 <- cor.test(Attack_Mean_B[-11],Proportion_B[-11])

plot(Attack_Mean_B,Proportion_B,xlab="特定回合中平均B个体啄击次数",ylab="特定回合中B个体进食比例",cex.lab=1.5)
abline(lm(Proportion_B~Attack_Mean_B))
text(3,0.7,labels=paste0("r = ",round(cor_P_B1$estimate,3),"\n",
                            "p = ",round(cor_P_B1$p.value,3)))

# 去除离群值11（整个育幼期亲鸟单次回巢喂食11次的情况只出现过一次；出现9次喂食的回合共有4次，此间B的攻击频率格外高）
points(Attack_Mean_B[11], Proportion_B[11], col = "red")
text(0.3,0,"11",col="red")
abline(lm(Proportion_B[-11]~Attack_Mean_B[-11]), col="red")
text(6,0.7,labels=paste0("r = ",round(cor_P_B2$estimate,3),"\n",
                            "p = ",round(cor_P_B2$p.value,3)),col="red")
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-16.png) 

##### 图8 B个体体啄击次数与进食比例的相关性

## B个体啄击次数与进食次数的线性相关情况

B增加攻击次数能保证一定比例的进食机会，但攻击次数与获得食物的量并无正相关。在喂食靠后的回合中（尤其是9、10回合），B个体需付出更多的努力来保持合理的进食比例，然而单次回巢喂食的平均次数为4.5±1.8次之间。


{% highlight r %}
Num.feed_B <- Prop[Prop$Group=="B","Ng"]
cor_Num_B1 <- cor.test(Attack_Mean_B,Num.feed_B) 
cor_Num_B2 <- cor.test(Attack_Mean_B[-11],Num.feed_B[-11]) 

plot(Attack_Mean_B,Num.feed_B,xlab="特定回合中B个体平均啄击次数",ylab="特定回合中B个体平均进食次数",cex.lab=1.5)
abline(lm(Num.feed_B~Attack_Mean_B))
text(9,85,labels=paste0("r = ",round(cor_Num_B1$estimate,3),"\n",
                            "p = ",round(cor_Num_B1$p.value,3)))
# 去除离群值11（整个育幼期亲鸟单次回巢喂食11次的情况只出现过一次；出现9次喂食的回合共有4次，此间B的攻击频率格外高）
points(Attack_Mean_B[11], Num.feed_B[11], col = "red")
text(0.3,0,"11",col="red")
abline(lm(Num.feed_B[-11]~Attack_Mean_B[-11]), col="red")
text(12,85,labels=paste0("r = ",round(cor_Num_B2$estimate,3),"\n",
                            "p = ",round(cor_Num_B2$p.value,3)),col="red")
{% endhighlight %}

![center](/image/2013-12-04-ibis-behavior-paper/unnamed-chunk-17.png) 

##### 图9 B个体体啄击次数与进食次数的相关性

