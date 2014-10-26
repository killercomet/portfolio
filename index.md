---
title       : Portfolio Computation
subtitle    : Computing Portfolios in R
author      : Guillermo Joaquin Corominas
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Portfolio computation

This program allows you to compute a weighted portfolio of two assets and evaluate the returns and the volatility associated with the individual assets as well as the weighted portfolio. 

This is particularly useful to evaluate how would a combination of two assets work, for example, when we are trying to reduce the total volatility of a portfolio, but at the same time we are trying to maintain a high return of our investment. In this case we can combine a low risk/low return asset with a high risk/high return asset and obtain a more balanced combined portfolio. 

To use the program you must: 

* Select the dates between you want to evaluate the performance of a weighted portfolio
* Select two different assets in the dropdown menus named "Asset 1" and "Asset 2"
* Select the weight allocated for the first asset (The weight allocated to the second asset is just
100% minus the weight allocated for the first asset)

--- .class #id 

## Example of computation 1/2. Loading data. 

For example, we can load data for APPLE and EXXON and evaluate the performance of both assets in a period of time between '2014-09-01' and '2014-10-24' and allocate a 25% for the first asset.

```r
date1 <- as.Date('2014-09-01') ;date2 <- as.Date('2014-10-24');weight <- 0.25
symbol1 <- read.csv("./APPL.csv", sep=","); symbol2 <- read.csv("./XOM.csv", sep=",")
symbols <- data.frame(date=symbol1$Date, symbol1=symbol1$Adj, symbol2=symbol2$Adj)
#Subset the data for the selected period
symbols$date <- as.Date(symbols$date)
Seq <- seq.Date(date1,date2,by="day"); symbolSub <- symbols[symbols$date %in% Seq,]
#Normalize data
symbolSub$symbol1 <- (symbolSub$symbol1/symbolSub$symbol1[length(symbolSub$symbol1)])
symbolSub$symbol2 <- (symbolSub$symbol2/symbolSub$symbol2[length(symbolSub$symbol2)])
symbolSub$weighted <- symbolSub$symbol1*weight + symbolSub$symbol2 * (1- weight)
```

--- .class #id 

## Example of computation 2/2. Computing volatility and returns. 

Now we compute the returns and volatility for both assets and the weighted portfolio. 

```r
#Compute returns 
return1 <- round(((symbolSub$symbol1[1] / symbolSub$symbol1[length(symbolSub$symbol1)])-1)* 100, 2) 
return2 <- round(((symbolSub$symbol2[1] / symbolSub$symbol2[length(symbolSub$symbol2)])-1)* 100, 2)
returnWeighted <- round(((symbolSub$weighted[1] / symbolSub$weighted[length(symbolSub$weighted)])-1)*100, 2)
#Compute sds
sd1 <- round(sd(symbolSub$symbol1)*100, 2)
sd2 <- round(sd(symbolSub$symbol2)*100, 2)
sdw <- round(sd(symbolSub$weighted)*100, 2)
```
As we see, the return of APPL is 1.86 and its volatility is 1.9. Exxon has a return of -4.06 and volatility of 2.59. The weighted portfolio returns -2.58 and has 2.1 volatility which is something in between but closer to EXXON because of the 75% allocation.

--- .class #id 

## Plotting the result

We can see the result of our computation by plotting using ggplot2. It is clear that our combined portfolio is located between both our assets. 

![plot of chunk unnamed-chunk-3](assets/fig/unnamed-chunk-3-1.png) 



