---
title: rkdb Examples
date: April 2020
description: Examples of ekdb
keywords: interface, kdb+, q, r, 
---

# <i class="fa fa-share-alt"></i> rkdb Examples

Some examples are demonstrated below where libaries are loaded and applied them to analyze data which was fetched from q.

```r
> #Load rkdb library
> library(rkdb)
> #test: run kdb+ on localhost:5000 for this
> test.rkdb()
> h <- open_connection("127.0.0.1", 5000, "testusername:testpassword")
> execute(h, "select count i by date from trade")
date x
1 2014-01-09 5125833
2 2014-01-10 5902700
3 2014-01-13 4419596
4 2014-01-14 4106744
5 2014-01-15 6156630
> # Setting TZ and retrieving time as timestamp simplifies time conversion
> Sys.setenv(TZ = "GMT")
> res <- execute(h,"select tradecount:count i, sum size, last price, vwap: size wavg price by time:0D00:05 xbar date+time from trade where date=2014.01.14,sym=`GOOG,time within 09:30 16:00")
> head(res)
                 time tradecount   size    price     vwap
1 2014-01-14 09:30:00       1471 142868 1132.000 1136.227
2 2014-01-14 09:35:00       1053  65599 1130.250 1132.674
3 2014-01-14 09:40:00       1019  77808 1132.422 1130.405
4 2014-01-14 09:45:00        563  39372 1130.846 1130.835
5 2014-01-14 09:50:00        586  38944 1129.312 1129.999
> plot(res$time ,res$price ,type="l",xlab="time",ylab="price")
```

which produces the plot in Figure 1:

![Last-traded price plot drawn from R](../../img/r/figure1.svg)
_Figure 1: Last-traded price plot drawn from R_

More comprehensive graphing is available in additional R packages, which can be freely downloaded.
For example, using the [xts](http://r-forge.r-project.org/projects/xts) package:

```r
> library(xts)
> # extract the HLOC buckets in 5-minute intervals
> res <- execute(h,"select high:max price,low:min price,open:first price, close:last price by time:0D00:05 xbar date+time from trade where date=2014.01.14,sym=`GOOG,time within 09:30 16:00")
> # create an xts object from the returned data frame
> # order on the time column
> resxts <-xts(res[,-1], order.by=res[,'time'])
> # create a vector of colours for the graph
> # based on the relative open and close prices
> candlecolors <- ifelse(resxts[,'close'] > resxts[,'open'], 'GREEN', 'RED')
> # display the candle graph with approrpiate labels
> plot.xts(resxts, type='candles', width=100, candle.col=candlecolors, bar.col='BLACK', xlab="time", ylab="price", main="GOOG HLOC")
```

produces the plot in Figure 2:

![Candlestick plot using xts package](../../img/r/figure2.png)
_Figure 2: Candlestick plot using xts package_

Another popular package is the [quantmod](http://www.quantmod.com) package which contains the `chartSeries` function.

```r
> library(quantmod)
> # extract the last closing price in 30 second buckets
> res <- execute(h,"select close:last price by time:0D00:00:30 xbar date+time from trade where date=2014.01.14,sym=`GOOG,time within 09:30 16:00")
> # create the closing price xts object
> closes <- xts(res[,-1], order.by=res[,'time'])
> # chart it. Add Bollinger Bands to the main graph
> # add additional Relative Strength Indicator and Rate Of Change graphs
> chartSeries(closes,theme=chartTheme("white"),TA=c(addBBands(),addTA(RSI( closes)),addTA(ROC(closes))))
```

This produces the plot shown in Figure 3:

![Chart example from quantmod package](../../img/r/figure3.svg)
_Figure 3: Chart example from quantmod package_

```r
> close_connection(h)
[1] 0
```

Further examples are provided in the `examples` folder of <i class="fab fa-github"></i> [KxSystems/rkdb](https://github.com/KxSystems/rkdb).