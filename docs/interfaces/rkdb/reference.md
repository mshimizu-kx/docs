---
title: rkdb User Guide
description: Lists functions available for use within the rkdb
date: April 2020
keywords: interface, kdb+, library, q, r
---

# :fontawesome-brands-r-project: rkdb User Guide

<div class="fusion" markdown="1">
:fontawesome-brands-superpowers: [Fusion for kdb+](../fusion.md)
</div>

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

Where

- `hostname` is a string denoting a domain or IP addressof the target kdb+ process
- `port` is an integer denoting a port of the target kdb+ process
- `username:password` is a string denoting a user name and password delimited by ":" to access to the target kdb+ process

```r
> h <- open_connection("127.0.0.1", 5000, "testusername:testpassword")
```

### close_connection

_Close a connection_

Syntax: `close_connection(connectionhandle)`

Where

- `connectionhandle` is a variable denoting a connection handle which was assigned a value returned by `open_connection`

```r
> close_connection(h)
[1] 0
```

### execute

_Execute a query on the specified connection handle_

Synatx:

- `execute(connectionhandle, query)` or 
- `execute(connectionhandle, query, arguments)`

Where 

- `connectionhandle` is a variable denoting a connection handle which was assigned a value returned by `open_connection`
     * &gt;0, executes the request synchronously, blocking the call
     * &lt;0, executes the request asynchronously; the result may arrive later
- `query` is a string denoting a query to be run on the connected kdb+ process
- `arguments` are variadic arguments to be passed to a query

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

Typical showcases are provided in the [Examples](examples.md) page.