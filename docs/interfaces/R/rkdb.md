---
title: rkdb, an interface giving R access to q/kdb+
description: Enable R to connect to kdb+ and extract data; enable q to connect to a remote instance of R via TCP/IP and invoke R routines remotely.
keywords: interface, kdb+, library, q, r
---

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

th {
  background-color: #368BC1;
  border: 1px solid #dddddd;
  text-align: left;
  padding: 6px;
}

td {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 6px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>

# <i class="fab fa-r-project"></i> rkdb User Guide

<div class="fusion" markdown="1">
<i class="fab fa-superpowers"></i> [Fusion for kdb+](../fusion.md)
</div>

Connects R to a kdb+ database to extract partially analyzed data into R
for further local manipulation, analysis and display.

## Install

Operating systems: tested and available for

-   <i class="fab fa-linux"></i> Linux (64-bit)
-   <i class="fab fa-apple"></i> macOS
-   <i class="fab fa-windows"></i> Windows (32-bit and 64-bit)

Download from <i class="fab fa-github"></i> [KxSystems/rkdb](https://github.com/KxSystems/rkdb) and follow the [installation instructions](https://github.com/KxSystems/rkdb#installation).

## Functions

The interface allows R to connect to a kdb+ database and send it a query, which can optionally return a result.

Three methods are available:

```txt
  //Open a connection to kdb+
  open_connection                  Open a connection to a q database. Multiple connections can be open at once

  //Close a connection to kdb+
  close_connection                 Close a connection

  //Execute a query
  execute                          Execute a query on the specified connection handle
```

### open_connection

_Open a connection to a q database. Multiple connections can be open at once_

Synatx: `open_connection(hostname, port, username:password)`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">hostname</tt></td>
          <td>string</td>
          <td>Domain or IP addressof the target kdb+ process</td>
     </tr>
     <tr>
          <td><tt style="font-size:110%">port</tt></td>
          <td>integer</td>
          <td>Port of the target kdb+ process</td>
     </tr>
     <tr>
          <td><tt style="font-size:110%">username:password</tt></td>
          <td>string</td>
          <td>User name and password delimited by ":" to access to the target kdb+ process</td>
     </tr>
</table>

```r
> h <- open_connection("127.0.0.1", 5000, "testusername:testpassword")
```

### close_connection

_Close a connection_

Syntax: `close_connection(connectionhandle)`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">connectionhandle</tt></td>
          <td>variable</td>
          <td>Connection handle which was opened by <tt style="font-size:110%">open_connection</tt></td>
     </tr>
</table>

```r
> close_connection(h)
[1] 0
```

### execute

_Execute a query on the specified connection handle_

Synatx:

- `execute(connectionhandle, query)` or 
- `execute(connectionhandle, query, arguments)`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">connectionhandle</tt></td>
          <td>variable</td>
          <td>Connection handle which was opened by <tt style="font-size:110%">open_connection</tt>.
          </br> &gt;0, executes the request synchronously, blocking the call
          </br> &lt;0, executes the request asynchronously; the result may arrive later</td>
     </tr>
     <tr>
          <td><tt style="font-size:110%">query</tt></td>
          <td>string</td>
          <td>Query to be run on the connected kdb+ process</td>
     </tr>
     <tr>
          <td><tt style="font-size:110%">arguments</tt></td>
          <td>variadic</td>
          <td>Arguments to be passed to a query. This parameter is variadic.</td>
     </tr>
</table>

```r
> execute(h, "{x, y}", "kdb", "plus")
> [1] "kdbplus"
> #or
> execute(h, "10 sublist select from trade where sym in `AAPL`MSFT, stop")
                                  time  sym price size stop cond ex
1  2020-04-21T08:45:00.245000000+00:00 AAPL 84.16   98 TRUE    W  N
2  2020-04-21T08:45:02.996000000+00:00 AAPL 84.11   72 TRUE    T  N
3  2020-04-21T08:45:04.134000000+00:00 MSFT 29.13   81 TRUE       N
4  2020-04-21T08:45:04.812000000+00:00 MSFT 29.13   62 TRUE    L  N
5  2020-04-21T08:45:09.955000000+00:00 AAPL 83.71   75 TRUE    J  N
6  2020-04-21T08:45:15.317000000+00:00 AAPL 84.44   21 TRUE    Z  N
7  2020-04-21T08:45:19.892000000+00:00 AAPL 85.39   23 TRUE    K  N
8  2020-04-21T08:45:20.710000000+00:00 AAPL 85.28   47 TRUE    J  N
9  2020-04-21T08:45:22.249000000+00:00 MSFT 28.58   43 TRUE    W  N
10 2020-04-21T08:45:23.287000000+00:00 MSFT 28.59   41 TRUE    T  N
> #or
> execute(-h, "a:42")
NULL
> execute(h, "a")
integer64
[1] 42
```

## Walk Through

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
head(res)
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
￼> close_connection(h)
[1] 0
```

Further examples are provided in the `examples` folder of <i class="fab fa-github"></i> [KxSystems/rkdb](https://github.com/KxSystems/rkdb).