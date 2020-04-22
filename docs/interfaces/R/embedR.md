---
title: embedR, an interface for calling R from q
author: Conor McCarthy
date: September 2019
description: embedR is an interface that allows the R programming language to be invoked by q programs
keywords: interface, kdb+, q, r, 
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

# embedR User Guide

<div class="fusion" markdown="1">
<i class="fab fa-superpowers"></i> [Fusion for kdb+](../fusion.md)
</div>

Invoke R from q

## Install

Work for both 32-bit and 64-bit kdb+. 

Pre-built packages are available from [here](https://github.com/KxSystems/embedR/releases/tag/v1.2.1) for:

-   <i class="fab fa-linux"></i> Linux
-   <i class="fab fa-apple"></i> macOS

If the appropriate build is not available on your target system, download from <i class="fab fa-github"></i> [KxSystems/embedR](https://github.com/KxSystems/embedR) and follow the installation instruction in `README.md`.

The `R_HOME` environment variable must be set prior to starting q.
To find out what that should be, run R from the Bash shell and see the result of `R.home()`

```r
> R.home()
[1] "/Library/Frameworks/R.framework/Resources"
```

and then set it accordingly in your environment; e.g. for macOS with a Bash shell

```bash
$export R_HOME=/Library/Frameworks/R.framework/Resources
```

Optional additional environment variables are `R_SHARE_DIR`, `R_INCLUDE_DIR`, `LD_LIBRARY_PATH` (for libR.so).

## Functions

A shared library can be loaded which brings R into the q memory space,
meaning all the R statistical routines and graphing capabilities can be invoked directly from q.
Using this method means data is not passed between remote processes.

The library has five methods:

```txt
  //Connection
  .rk.open:            Initialise embedR. Optional to call. Allows to set verbose mode as Ropen 1
  .rk.close:           Close internal R connection

  //Execution
  .rk.exec:            Run an R command, do not return a result
  .rk.get:             Run an R command, return the result to q
  .rk.set:             Set a variable in the R memory space

  //Graphic
  .rk.dev              Open plot window with noRStudioGD=TRUE (Normally R will open a new device automatically)
  .rk.off              Close plot window

  //Utility
  .rk.install          Install package in embeded R process over the connection

```

### Connection

#### .rk.open

_Initialise embedR. Optional to call. Allows to set verbose mode as Ropen 1_

Syntax: `.rk.open[signal]`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">signal</tt></td>
          <td>integer</td>
          <td>0 or empty: quiet</br> 1: verbose mode</td>
     </tr>
</table>

```q
q).rk.open[]
q)//or
q).rk.open[1]
```

<p style="font-size:80%"> <b>Note</b>: As of version 2.0 <b>Ropen</b>  was migrated to <b>.rk.open</b>. <b>Ropen</b> will be depricated in version 2.1.</p>

#### .rk.close

_Close internal R connection_

Syntax: `.rk.close[]`

<p style="font-size:80%"> <b>Note</b>: As of version 2.0 <b>Rclose</b> was migrated to <b>.rk.close</b>. <b>Rclose</b> will be depricated in version 2.1.</p>

### Execution

#### .rk.exec

_Run an R command, do not return a result_

Synatax: `.rk.exec[command]`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">command</tt></td>
          <td>string</td>
          <td>R expression to execute in R process</td>
     </tr>
</table>

```q
q).rk.exec "Sys.setenv(TZ = 'UTC')"
q).rk.exec "library(xts)"
Loading required package: zoo

Attaching package: ‘zoo’

The following objects are masked from ‘package:base’:

    as.Date, as.Date.numeric
```

#### .rk.get

_Run an R command, return the result to q_

Synatax: `.rk.get[rexp]`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">rexp</tt></td>
          <td>string</td>
          <td>R expression to execute in R process</td>
     </tr>
</table>

```q
q).rk.exec "today <- as.Date('2020-04-01')"
q).rk.get "today"
,2019.04.01
```

#### .rk.set

_Set a variable in the R memory space_

Synatax: `.rk.set[rvar; qvar]`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">rvar</tt></td>
          <td>string</td>
          <td>Name of R variable used in R process</td>
     </tr>
          <tr>
          <td><tt style="font-size:110%">qvar</tt></td>
          <td>object</td>
          <td>Q object used to assign to the R variable in R process</td>
     </tr>
</table>

```q
q).rk.set["mnth"; `month$/:2010.01.29 2020.04.02]
q).rk.get "mnth"
2010.01 2020.04m
```

### Graphic

#### .rk.new

_Open plot window with noRStudioGD=TRUE (Normally R will open a new device automatically)_

Syntax: `.rk.new[]`

<p style="font-size:80%"> <b>Note</b>: As of version 2.0 <b>Rnew</b> was migrated to <b>.rk.new</b>. <b>Rnew</b> will be depricated in version 2.1.</p>

#### .rk.off

_Close plot window_

To close the graphics window, use `dev.off()` rather than the close button on the window.

<p style="font-size:80%"> <b>Note</b>: As of version 2.0 <b>Roff</b> was migrated to <b>.rk.off</b>. <b>Roff</b> will be depricated in version 2.1.</p>

### Utility

#### .rk.install

_Install package in embeded R process over the connection_

Syntax: `.rk.install[package]`

<table>
     <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Explanation</th>
     </tr>
     <tr>
          <td><tt style="font-size:110%">package</tt></td>
          <td>string</td>
          <td>R package name to install in R process</td>
     </tr>
</table>

```q
q).rk.install["psy"]
```

<p style="font-size:80%"> <b>Note</b>:
<ul>
  <li style="font-size:80%">As of version 2.0 <b>Rinstall</b> was migrated to <b>.rk.install</b>. <b>Rinstall</b> will be depricated in version 2.1.</li>
  <li style="font-size:80%">We strongly recommend you install packages by independent R process.</li>
</ul>
</p>

## Walk Through

An example is outlined below, using q to subselect some data and then passing it to R for graphical display.

```q
q)select count i by date from trade
date      | x
----------| --------
2014.01.07| 29205636
2014.01.08| 30953246
2014.01.09| 30395962
2014.01.10| 29253110
2014.01.13| 32763630
2014.01.14| 29721162
2014.01.15| 30035729
..

q)//extract mid prices in 5 minute bars
q)mids:select mid:last .5*bid+ask by time:0D00:05 xbar date+time from quotes where date=2014.01.17,sym=`IBM,time within 09:30 16:00
q)mids
time                         | mid
-----------------------------| --------
2014.01.15D09:30:00.000000000| 185.92
2014.01.15D09:35:00.000000000| 185.74
2014.01.15D09:40:00.000000000| 186.11
2014.01.15D09:45:00.000000000| 186.36
2014.01.15D09:50:00.000000000| 186.5
2014.01.15D09:55:00.000000000| 186.98
2014.01.15D10:00:00.000000000| 187.45
2014.01.15D10:05:00.000000000| 187.48  
2014.01.15D10:10:00.000000000| 187.66  
..

q)Load in R
q)\l init.q
q)//Pass the table into the R memory space
q).rk.set["mids";mids]
q)//Graph it
q).rk.exec["plot(mids$time, mids$mid, type='l', xlab='time', ylab='price')"]
```

This will produce a plot as shown in Figure 4: 

![Quote mid price plot drawn from q](../../img/r/figure4.svg)  
_Figure 4: Quote mid price plot drawn from q_

```q
q)//Save as a PDF file
q).rk.exec "pdf('test.pdf')"
q)//Close plot window
q).rk.off[]
```

If the q and R installations are running remotely from the user on a Linux machine, the graphics can be seen locally using X11 forwarding over SSH.

Aside from using R’s powerful graphics, this mechanism also allows you to call R analytics from within q.

Note that R’s timezone setting affects date transfers between R and q. For example, in the R server:

```q
q).rk.exec "Sys.setenv(TZ='GMT')"
q).rk.get "date()"
"Fri Feb  3 06:33:43 2012"
q).rk.exec "Sys.setenv(TZ='EST')"
q).rk.get "date()"
"Fri Feb  3 01:33:57 2012"
```

<i class="far fa-hand-point-right"></i>
Knowledge Base: [Timezones and Daylight Saving Time](../../kb/timezones.md)

Further examples are provided in the `examples` folder of <i class="fab fa-github"></i> [KxSystems/embedR](https://github.com/KxSystems/embedR).