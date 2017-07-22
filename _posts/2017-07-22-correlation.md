---
layout:     post
title:      "简单相关分析"
subtitle:   "correlation analysis"
date:       2017-07-22 20:15:00
author:     "207统计工作室"
header-img: "img/in-post/post-bg/article1.jpeg"
header-mask: 0.3
catalog:    true
tags:
    - correlation
    - corrgram
    - corrplot
---


相关关系是一种研究随机变量间相关关系的统计方法。散点图和相关图是常见的相关分析方法。

散点图
======

散点图矩阵
----------

下面以R自带数据集*iris*为例，分析*Sepal.Length*,*Sepal.Width*,*Petal.Length*和*Petal.Width*四个变量之间的关系。使用*pairs()*函数绘制散点图矩阵，代码如下：

``` r
pairs(~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width,data = iris,
      main = "散点图矩阵")
```

![](/img/in-post/correlation/unnamed-chunk-1-1.png) 

通过散点图帐数据分布，可以看出*Petal.Length*和*Petal.Width*二者之间的相关性较强。

``` r
library(car)
scatterplotMatrix(~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width|Species,data = iris)
```

![](/img/in-post/correlation/unnamed-chunk-2-1.png)

相关图
======

将不同变量之间的相关关系通过可视化的方法展示出来即相关图。

相关矩阵图
----------

`corrgram`中的`corrgram()`函数和`corrplot`包中的`corrplot()`函数均可绘制相关矩阵图。一般用法如下所示：

``` r
library(corrgram)
# 1、排序处理
corrgram(mtcars,order = TRUE) 
```

![](/img/in-post/correlation/unnamed-chunk-3-1.png)

``` r
# 2、设置三角面板形状
corrgram(mtcars,order = TRUE, lower.panel = panel.shade,upper.panel = panel.pie)
```

![](/img/in-post/correlation/unnamed-chunk-4-1.png)

``` r
# 3、只显示下三角面板
corrgram(mtcars,order = TRUE, lower.panel = panel.shade,upper.panel = NULL)
```

![](/img/in-post/correlation/unnamed-chunk-5-1.png)

``` r
# 4、调整面板颜色
corrgram(mtcars,order = TRUE, lower.panel = panel.shade,upper.panel = panel.pie,
         col.regions = colorRampPalette(c("darkgoldenrod4","burlywood1","white",
                                          "darkkhaki","darkgreen")))
```

![](/img/in-post/correlation/unnamed-chunk-6-1.png)

使用`corrplot()`函数绘制的示例如下：

``` r
library(corrplot)
# 1、使用不同的method绘制相关矩阵图
methods <- c("circle","square","ellipse","pie","shade","color")
par(mfrow = c(2,3))
M <- cor(mtcars)
t0 <- mapply(function(x){corrplot(M, method = x, order = "AOE")},methods)
```

![](/img/in-post/correlation/unnamed-chunk-7-1.png)

``` r
par(mfrow = c(1,1))
```

``` r
# 2、设置method = color 绘制热力矩阵图
corrplot(cor(mtcars), method = "color", order = "AOE", tl.col = "black",
         tl.srt = 45, addCoef.col = "black",
         col = colorRampPalette(c("#7F0000","red","#FF7F00","yellow","white",
                                  "cyan","#007FFF","blue","#00007F"))(20))
```

![](/img/in-post/correlation/unnamed-chunk-8-1.png)

``` r
# 3、绘制上下三角及不同色彩的相关矩阵图
library(RColorBrewer)
par(mfrow = c(2,2))
corrplot(cor(mtcars),type = "lower")
corrplot(cor(mtcars),type = "lower", order = "hclust",
         col = brewer.pal(n=8, name = "RdYlBu"))
corrplot(cor(mtcars),type = "upper", order = "AOE",
         col = c("black","white"),bg = "lightblue")
corrplot(cor(mtcars),type = "upper", order = "FPC",
         col = brewer.pal(n=8, name = "PuOr"))
```

![](/img/in-post/correlation/unnamed-chunk-9-1.png)

``` r
par(mfrow = c(1,1))
```
