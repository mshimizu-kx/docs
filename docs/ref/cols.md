---
keywords: cols, column, kdb+, q, table, xcol, xcols
---

# `cols`, `xcol`, `xcols`

_Table columns_



## `cols`

_Column names of a table_

Syntax: `cols x`, `cols[x]`

Where `x` is a table, returns as a symbol vector its column names. 

`x` can be passed by reference or by value.

```q
q)\l trade.q
q)cols trade            /value
 `time`sym`price`size
q)cols`trade            /reference
 `time`sym`price`size
```


## `xcol`

_Rename table columns_

Syntax: `x xcol y`, `xcol[x;y]`

Where 

-   `y` is a table (passed by value) 
-   `x` is a symbol vector of length no greater than `count cols y`

returns `y` with its first `count x` columns renamed. 

```q
q)\l trade.q
q)cols trade
`time`sym`price`size
q)`Time`Symbol xcol trade           / rename first two columns
Time         Symbol price size
------------------------------
09:30:00.000 a      10.75 100
q)trade:`Time`Symbol`Price`Size xcol trade  / rename all and assign
q)cols trade
`Time`Symbol`Price`Size
```



## `xcols`

_Reorder table columns_

Syntax: `x xcols y`, `xcols[x;y]`

Where 

-   `y` is a simple table (passed by value) 
-   `x` is a symbol vector of some or all of `y`’s column names

returns `y` with `x` as its first column/s.

```q
q)\l trade.q
q)cols trade
`time`sym`price`size
q)trade:xcols[reverse cols trade;trade] / reverse cols and reassign trade
q)cols trade
`size`price`sym`time
q)trade:`sym xcols trade                / move sym to the front
q)cols trade
`sym`size`price`time
```



<i class="far fa-hand-point-right"></i> 
[`meta`](meta.md)  
.Q: [`.Q.V`](dotq.md#qv-table-to-dict) (table to dictionary)  
Basics: [Dictionaries & tables](../basics/dictsandtables.md), 
[Metadata](../basics/metadata.md) 