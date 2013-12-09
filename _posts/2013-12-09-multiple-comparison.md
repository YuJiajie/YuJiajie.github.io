---
layout: post
keywords: multiple comparison
description: blog
title: 方差分析的多重比较
categories: [R笔记]
tags: [R 多重比较]
group: archive
icon: bullhorn
---
> 用到[multcomp] (http://cran.r-project.org/web/packages/multcomp/index.html)包,并非方差分析完全教程

# 安装&加载

{% highlight r %}
install.packages("multcomp")
{% endhighlight %}



{% highlight r %}
library(multcomp)
{% endhighlight %}


# 数据

{% highlight r %}
data(warpbreaks)
str(warpbreaks)
{% endhighlight %}



{% highlight text %}
## 'data.frame':	54 obs. of  3 variables:
##  $ breaks : num  26 30 54 25 70 52 51 26 67 18 ...
##  $ wool   : Factor w/ 2 levels "A","B": 1 1 1 1 1 1 1 1 1 1 ...
##  $ tension: Factor w/ 3 levels "L","M","H": 1 1 1 1 1 1 1 1 1 2 ...
{% endhighlight %}


# 单因素方差分析

> 单因素（tension）三水平（L，M，H）方差分析

## 数据分布
#### Shapiro-Wilk 正态性检验

{% highlight r %}
with(warpbreaks, tapply(breaks, tension, shapiro.test))
{% endhighlight %}



{% highlight text %}
## $L
## 
## 	Shapiro-Wilk normality test
## 
## data:  X[[1L]]
## W = 0.9058, p-value = 0.07265
## 
## 
## $M
## 
## 	Shapiro-Wilk normality test
## 
## data:  X[[2L]]
## W = 0.9428, p-value = 0.3232
## 
## 
## $H
## 
## 	Shapiro-Wilk normality test
## 
## data:  X[[3L]]
## W = 0.9191, p-value = 0.1247
{% endhighlight %}

#### 条形图

{% highlight r %}
with(warpbreaks, tapply(breaks, tension, function(x) hist(x, freq = F)))
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-51.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-52.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-53.png) 

y <- rt(200, df = 5)
qqnorm(y); qqline(y, col = 2)
qqplot(y, rt(300, df = 5))

## <a name="A2">方差分析表</a>
> <a href="#A3">对比双因素结果</a>


{% highlight r %}
library(xtable)
amod <- aov(breaks ~ tension, data = warpbreaks)
anov.tb <- xtable(summary(amod), caption = "方差分析表")
print(anov.tb, type = "html", caption.placement = getOption("xtable.caption.placement", 
    "top"))
{% endhighlight %}

<!-- html table generated in R 3.0.0 by xtable 1.7-1 package -->
<!-- Mon Dec 09 11:27:55 2013 -->
<TABLE border=1>
<CAPTION ALIGN="top"> 方差分析表 </CAPTION>
<TR> <TH>  </TH> <TH> Df </TH> <TH> Sum Sq </TH> <TH> Mean Sq </TH> <TH> F value </TH> <TH> Pr(&gt F) </TH>  </TR>
  <TR> <TD> tension     </TD> <TD align="right"> 2 </TD> <TD align="right"> 2034.26 </TD> <TD align="right"> 1017.13 </TD> <TD align="right"> 7.21 </TD> <TD align="right"> 0.0018 </TD> </TR>
  <TR> <TD> Residuals   </TD> <TD align="right"> 51 </TD> <TD align="right"> 7198.56 </TD> <TD align="right"> 141.15 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
   </TABLE>

## 多重比较

> Tukey's multiple comparisons tests: The "Honestly Significantly Different" (HSD) test proposed by the 
statistician John Tukey is based on what is called the "studentized
range distribution."    
> M与L差异显著，H与L差异极显著，H与M差异不显著

#### multcomp包

{% highlight r %}
tuk <- glht(amod, linfct = mcp(tension = "Tukey"))
summary(tuk)
{% endhighlight %}



{% highlight text %}
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Tukey Contrasts
## 
## 
## Fit: aov(formula = breaks ~ tension, data = warpbreaks)
## 
## Linear Hypotheses:
##            Estimate Std. Error t value Pr(>|t|)   
## M - L == 0   -10.00       3.96   -2.53   0.0384 * 
## H - L == 0   -14.72       3.96   -3.72   0.0014 **
## H - M == 0    -4.72       3.96   -1.19   0.4631   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## (Adjusted p values reported -- single-step method)
{% endhighlight %}


#### 基础包（stats）

{% highlight r %}
(tuk.base <- TukeyHSD(amod, "tension"))
{% endhighlight %}



{% highlight text %}
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = breaks ~ tension, data = warpbreaks)
## 
## $tension
##        diff    lwr     upr  p adj
## M-L -10.000 -19.56 -0.4402 0.0385
## H-L -14.722 -24.28 -5.1624 0.0014
## H-M  -4.722 -14.28  4.8376 0.4631
{% endhighlight %}



### 置信区间图

#### multcomp包

{% highlight r %}
plot(print(confint(tuk)))
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-9.png) 


#### 基础包（stats）

{% highlight r %}
plot(tuk.base)
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-10.png) 


## 残差分布

> 残差 vs 拟合值可以检查残差是否有趋势或残差是否满足一致性；正态Q-Q图检查残差是否满足正态性。


{% highlight r %}
plot(amod, which = 1:2)
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-111.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-112.png) 


# 双因素方差分析

> 双因素（tension和wool）多水平（t:L，M，H; w:A，B）方差分析

## 数据分布
> 多因素方差分析中，数据要求满足因素水平交叉内的正态性，然而水平交叉内的自由度往往较小，直接的正态性检查不可操作，因此一般由残差的正态性来反映,见<a href="#A1">残差分布</a>。

#### 条形图

{% highlight r %}
with(warpbreaks, for (i in c("L", "H", "M")) {
    hist(warpbreaks[wool == "A" & tension == i, "breaks"], xlab = paste0("breaks A", 
        i), main = NULL)
})
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-121.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-122.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-123.png) 



{% highlight r %}
with(warpbreaks, for (i in c("L", "H", "M")) {
    hist(warpbreaks[wool == "B" & tension == i, "breaks"], xlab = paste0("breaks B", 
        i), main = NULL)
})
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-131.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-132.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-133.png) 



## <a name="A3">方差分析表</a>
> <a href="#A2">比较单因素</a>,双因素中tension的p值小一点点，wool不显著。


{% highlight r %}
library(xtable)
amod2 <- aov(breaks ~ tension + wool, data = warpbreaks)
anov.tb <- xtable(summary(amod2), caption = "方差分析表")
print(anov.tb, type = "html", caption.placement = getOption("xtable.caption.placement", 
    "top"))
{% endhighlight %}

<!-- html table generated in R 3.0.0 by xtable 1.7-1 package -->
<!-- Mon Dec 09 11:27:56 2013 -->
<TABLE border=1>
<CAPTION ALIGN="top"> 方差分析表 </CAPTION>
<TR> <TH>  </TH> <TH> Df </TH> <TH> Sum Sq </TH> <TH> Mean Sq </TH> <TH> F value </TH> <TH> Pr(&gt F) </TH>  </TR>
  <TR> <TD> tension     </TD> <TD align="right"> 2 </TD> <TD align="right"> 2034.26 </TD> <TD align="right"> 1017.13 </TD> <TD align="right"> 7.54 </TD> <TD align="right"> 0.0014 </TD> </TR>
  <TR> <TD> wool        </TD> <TD align="right"> 1 </TD> <TD align="right"> 450.67 </TD> <TD align="right"> 450.67 </TD> <TD align="right"> 3.34 </TD> <TD align="right"> 0.0736 </TD> </TR>
  <TR> <TD> Residuals   </TD> <TD align="right"> 50 </TD> <TD align="right"> 6747.89 </TD> <TD align="right"> 134.96 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
   </TABLE>

## 多重比较

> tension同样是M与L差异显著，H与L差异极显著，H与M差异不显著    

#### multcomp包

{% highlight r %}
tuk2 <- glht(amod2, linfct = mcp(tension = "Tukey"))
summary(tuk2)
{% endhighlight %}



{% highlight text %}
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Tukey Contrasts
## 
## 
## Fit: aov(formula = breaks ~ tension + wool, data = warpbreaks)
## 
## Linear Hypotheses:
##            Estimate Std. Error t value Pr(>|t|)   
## M - L == 0   -10.00       3.87   -2.58   0.0337 * 
## H - L == 0   -14.72       3.87   -3.80   0.0012 **
## H - M == 0    -4.72       3.87   -1.22   0.4474   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## (Adjusted p values reported -- single-step method)
{% endhighlight %}


#### 基础包（stats）

{% highlight r %}
(tuk.base2 <- TukeyHSD(amod2, "tension"))
{% endhighlight %}



{% highlight text %}
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = breaks ~ tension + wool, data = warpbreaks)
## 
## $tension
##        diff    lwr     upr  p adj
## M-L -10.000 -19.35 -0.6466 0.0336
## H-L -14.722 -24.08 -5.3688 0.0011
## H-M  -4.722 -14.08  4.6312 0.4474
{% endhighlight %}



### 置信区间图

#### multcomp包

{% highlight r %}
plot(print(confint(tuk2)))
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-17.png) 


#### 基础包（stats）

{% highlight r %}
plot(tuk.base2)
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-18.png) 


## <a name="A1">残差分布</a>

> 残差 vs 拟合值可以检查残差是否有趋势或残差是否满足一致性；正态Q-Q图检查残差是否满足正态性。


{% highlight r %}
plot(amod2, which = 1:2)
{% endhighlight %}

![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-191.png) ![center](/image/2013-12-09-multiple-comparison/unnamed-chunk-192.png) 











