# Glossary


> Ontology asks, _What exists?_,  
> to which the answer is _Everything_.  
> — W.V.O. Quine, _Word and Object_



## Aggregate function

A function that reduces its argument, typically a list to an atom, e.g. `sum`


## Ambivalent 

A map that may be applied to a variable number arguments is ambivalent. For example, a matrix, the operator `@`, or the extension `+/`. 


## Apply

As in _apply a function to its arguments_:  evaluate a function on values corresponding to its arguments.  
<i class="far fa-hand-point-right"></i> [Application](application.md)


## Argument

In the expression `10%4` the operator `%` is evaluated on the arguments 10 and 4. 10 is the _left argument_ and 4 is the _right argument_.


By extension, the first and second arguments of a binary function are called its left argument and right argument regardless of whether it is applied infix.
In the expression `%[10;4]` 10 and 4 are still referred to as the left and right arguments. 


By extension, where a function has rank >2, its left argument is its first argument, and its _right arguments_ are the remaining arguments.


Correspondingly, the _left domain_ and _right domain_ of a binary function are the domains of its first and second arguments, regardless of whether or not the function may be applied infix. 


By extension, where a function has rank >2, its _left domain_ is the domain of its first argument, and its _right domains_ are the domains of the remaining arguments.


The terminology generalises to maps. 

-   The left domain of a matrix `m` is `til count m`.
-   The right domain of a matrix is `til count first m`.
-   The right domains of a list `m` of depth `n` are `1_n{til count first x}\m`. <!-- FIXME Check -->


Because unary functions may be applied prefix, the single argument of a unary function is sometimes referred to as its _right argument_. 


## Argument list

A pair of square brackets enclosing zero or more items separated by semicolons. <pre><code class="language-q">%[10;4]  / % applied to argument list [10;4]</code></pre>


## Atom

A single instance of a [datatype](datatypes.md), eg `42`, `"a"`, `1b`, `2012.09.15`. The [`type`](../ref/type.md) of an atom is always negative. 


## Atomic function 

An atomic function is a uniform function such that for `r:f[x]`  `r[i]~f x[i]` is true for all `i`, e.g. `signum`. A function `f` is atomic if `f` is identical to `f'`. 

<i class="far fa-hand-point-right"></i>
[Atomic functions](atomic.md)


## Attribute

Attributes are metadata that apply to lists of special form. 
They are often used on a dictionary domain or a table column to reduce storage requirements or to speed retrieval.

<i class="far fa-hand-point-right"></i> 
[`#` Set Attribute](../ref/set-attribute.md)


## Binary  

A map of rank 2, i.e. a function that takes 2 arguments, or a list of depth ≥2.
(The terms _dyad_ and _dyadic_ are now deprecated.)


## Bracket notation

Applying a map to its argument/s or indexes by writing it to the left of an argument list, e.g. `+[2;3]` or `count["zero"]`.

<i class="far fa-hand-point-right"></i> 
[Application](application.md)


## Character constant

A character constant is defined by entering the characters between double-quotes, as in `"abcdefg"`. If only one character is entered the constant is an atom, otherwise the constant is a list. For example, `"a"` is an atom. The expression `enlist "a"` is required to indicate a one character list. 

<i class="far fa-hand-point-right"></i> 
[Escape sequences](#escape-sequences) for entering non-graphic characters in character constants.


## Character vector

A character vector is a simple list whose items are all character atoms. When displayed in a session, it appears as a string of characters surrounded by double-quotes, as in: `"abcdefg"`, not as individual characters separated by semicolons and surrounded by parentheses (that is, not in list notation). 

When a character vector contains only one character, the display is distinguished from the atomic character by prepending a comma, as in `,"x"`.

_String_ is another name for character vector.


## Comment

Characters ignored by the interpreter.

<i class="far fa-hand-point-right"></i>
[Comment syntax](syntax.md#comments)


## Comparison tolerance

Because floating-point values resulting from computations are usually only approximations to the true mathematical values, the Equal operator is defined so that `x = y` is `1b` (true) for two floating-point values that are either near one another or identical. 

<i class="far fa-hand-point-right"></i> 
[Precision](precision.md)


## Conform

Lists, dictionaries and tables conform if they are either atoms or have the same count.

<i class="far fa-hand-point-right"></i> 
[Conformability](conformable.md)


## Console

Console refers to the source of messages to q and their responses that are typed in a q session.


## Control word

Control words `do`, `if`, and `while` interrupt the usual evaluation rules, e.g. by omitting expressions, terminating evaluation.

<i class="far fa-hand-point-right"></i> 
[Evaluation control](control.md)


## Count 

The number of items in a list, keys in a dictionary or rows in a table. The count of an atom is 1.


## Depth

The depth of a list is the number of levels of nesting. For example, an atom has depth 0, a list of atoms has depth 1, a list of lists of atoms has depth 2, and so on. 

The following function computes the depth of any data object:

```q
q)depth:{$[0>type x; 0; 1 + max depth'[x]]}
```

That is, an atom has depth 0 and a list has depth equal to 1 plus the maximum depth of its items. 

```q
q)depth 10             / atom
0
q)depth 10 20          / vector
1
q)depth (10 20;30)     / list
2
```


## Dictionary

A dictionary is a map from a list of keys to a list of values. (The keys should be unique, though q does not enforce this.) The values of a dictionary can be any data structure.

```q
q)/4 keys and 4 atomic values
q)`bob`carol`ted`alice!42 39 51 44
bob  | 42
carol| 39
ted  | 51
alice| 44
q)/2 keys and 2 list values
q)show kids:`names`ages!(`bob`carol`ted`alice;42 39 51 44)
names| bob carol ted alice
ages | 42  39    51  44
```

<i class="far fa-hand-point-right"></i> 
[`!` Dict](../ref/dict.md)


## Domain

The domain of a function is the complete set of possible values of its argument.  
<i class="far fa-hand-point-right"></i> 
intmath.com: [Domain and range](http://www.intmath.com/functions-and-graphs/2a-domain-and-range.php)


## Empty list

The generic empty list has no items, has count 0, and is denoted by `()`. The empty character vector may be written `""`, the empty integer vector `0#0`, the empty floating-point vector `0#0.0`, and the empty symbol vector ``0#` `` or `` `$()``. 

The distinction between `()` and the typed empty lists is relevant to certain operators (e.g. Match) and also to formatting data on the screen.


## Enumeration

A representation of a list as indexes of the items in its nub or another list.  
<i class="far fa-hand-point-right"></i> 
[Enumerations](enumerations.md)


## Entry

The items of a dictionary are its entries. 
Each entry consists of a key and a corresponding value. 


## Escape sequence

An escape sequence is a special sequence of characters representing a character atom. An escape sequence usually has some non-graphic meaning, for example the tab character. An escape sequence can be entered in a character constant and displayed in character data. 


## Expression block, expression list

A pair of square brackets enclosing zero or more expressions separated by semicolons. 


## Extender, extension

An extender is a higher-order operator. They take maps as arguments and return functions known as _extensions_, which extend the application of the maps.

All the extenders are unary except Compose, which is binary. The unary extenders are the only operators that can be applied postfix, and almost invariably are.

<i class="fa-hand-point-right"></i>
Reference: [Extenders](../ref/extenders.md)


## Finite-state machine

A dictionary or list represents a finite-state machine when its values (dictionary) or items (list) can be used to index it. For example:

```q
q)show l:-10?10
1 8 5 7 0 3 6 4 2 9             / all items are also indexes
q)yrp                           / a European tour
from   to     wp
----------------
London Paris  0
Paris  Genoa  1
Genoa  Milan  1
Milan  Vienna 1
Vienna Berlin 1
Berlin London 0
q)show route:(!/)yrp`from`to    / finite-state machine
London| Paris
Paris | Genoa
Genoa | Milan
Milan | Vienna
Vienna| Berlin
Berlin| London
```


## Function

A map from domain/s to range defined by an algorithm.

Operators, keywords, extensions, compositions, projections and lambdas are all functions. 

<i class="far fa-hand-point-right"></i> 
[`.Q.res`](../ref/dotq.md#qres-k-words) returns a list of keywords



## Function atom

A function can appear in an expression as data, and not be subject to immediate evaluation when the expression is executed, in which case it is an atom. For example:

```q
q)f: +            / f is assigned Add 
q)(f;102)         / an item in a list
+
102
```


## Handle

A handle is a symbol holding the name of a global variable, which is a node in the K-tree. For example, the handle of the name `a_c` is `` `a_c``. The term _handle_ is used to point out that a global variable is directly accessed. Both of the following expressions amend `x`:

```q
x: .[ x; i; f; y]
   .[`x; i; f; y]
```

In the first, referencing `x` as the first argument causes its entire value to be constructed, even though only a small part may be needed. In the second, the symbol `` `x`` is used as the first argument. In this case, only the parts of `x` referred to by the index `i` will be referenced and reassigned. The second case is usually more efficient than the first, sometimes significantly so. 

Where `x` is a directory, referencing the global variable `x` causes the entire dictionary value to be constructed, even though only a small part of it may be needed. Consequently, in the description of Amend, the symbol atoms holding global variable names are referred to as handles.


## Identity element

For function `f` the value `x` such that `y~f[x;y]` for any `y`. 

Q knows the identity elements of some functions, e.g. `+` (zero), but not others, e.g. {x+y} (also zero). 

<i class="far fa-hand-point-right"></i> 
[Ambivalence](ambivalence.md)


## Infix

Applying an operator by writing it between its arguments, e.g.  
`2+3`  applies `+` to 2 and 3


## Item, list item

A member of a list: can be any function or data structure.


## K-tree

The K-tree is the hierarchical name space containing all global variables created in a session. The initial state of the K-tree when kdb+ is started is a working directory whose absolute path name is `` `.`` together with a set of other top-level directories containing various utilities. The working directory is for interactive use and is the default active, or current, directory. 

An application should define its own top-level directory that serves as its logical root, using a name which will not conflict with any other top-level application or utility directories present. Every subdirectory in the K-tree is a dictionary that can be accessed like any other variable, simply by its name. 


## Keyed table

See [Table](#table).


## Lambda

Functions are defined in the _lambda notation_: an optional signature followed by a list of expressions, separated by semicolons, and all embraced by curly braces, e.g.  
`{[a;b](a*a)+(b*b)+2*a*b}`. 

A defined function is also known as a _lambda_.

<i class="far fa-hand-point-right"></i> 
[Lambda notation](function-notation.md)


## Left argument

See _Argument_


## Left-atomic function

A left-atomic function `f` is a binary `f` that is atomic in its left, or first, argument. That is, for every valid right argument `y`, the unary `f[;y]` is atomic.


## Left domain

See _Argument_


## List

An array, its items indexed by position.

<i class="far fa-hand-point-right"></i>
[List notation](syntax.md#list-notation)


## Map

A function, list, or dictionary. 
<!-- <i class="far fa-hand-point-right"></i> 
[Maps](../tutorials/uq/maps.md)
 -->

## Matrix

A list in which all items are lists of the same count.


## Name, namespace

A [namespace](https://en.wikipedia.org/wiki/Namespace) is a container or context within which a name resolves to a unique value. 
Namespaces are children of the _default namespace_ and are designated by a dot prefix. 
Names in the default namespace have no prefix. 
The default namespace of a q session is parent to multiple namespaces, e.g. `.h`, `.Q` and `.z`. 
(Namespaces with 1-character names – of either case – are reserved for use by Kx.)

```q
q).z.p                         / UTC timestamp
2017.02.01D14:58:38.579614000
```

Namespaces are dictionaries.

```q
q)v:5
q).ns.v:6
q)`.[`v]      / value of v in root namespace
5
q)`.ns[`v]    / value of v in ns
6
q)`. `v       / indexed by juxtaposition
5
q)`.ns `v`v
6 6
q)`.`.ns@\:`v
5 6
```


## Native

A synonym for _primitive_.


## Nub

The unique items of a list.

<i class="far fa-hand-point-right"></i>
Reference: [`distinct`](../ref/distinct.md)


## Null

Null is the value of an unspecified item in a list formed with parentheses and semicolons. For example, null is the item at index position 2 of ``(1 2;"abc";;`xyz)``. 

Null is an atom; its value is `::` <!-- , or `first()` -->. Nulls have special meaning in the right argument of the operator Index and in the bracket form of function application.


## Nullary

A function of rank 0, i.e. that takes no arguments.


## Operator

A primitive binary function that may be applied infix as well as prefix, e.g. `+`, `&`.  

<i class="far fa-hand-point-right"></i> 
[Application](application.md)


## Postfix

Applying an adverb to its argument by writing it to the right, e.g. `+/` applies `/` to `+`. (Not to be confused with projecting an operator on its left argument.)


## Prefix

Prefix notation applies a unary map `m` to its argument or indices `x`; i.e. `m x` is equivalent to `m[x]`. 

<i class="far fa-hand-point-right"></i> 
[Application](application.md)


## Primitive

Defined in the q language.


## Project, projection

A function passed fewer arguments than its rank projects those arguments and returns a projection: a function of the unspecified argument/s. 

<i class="far fa-hand-point-right"></i> 
[Projection](syntax.md#projection)


## Quaternary

A map with rank 4.


## Range 

The range of a function is the complete set of all its possible resulting values. 

<i class="far fa-hand-point-right"></i> 
intmath.com: [Domain and range](http://www.intmath.com/functions-and-graphs/2a-domain-and-range.php)


## Rank

Of a **function**, the number of arguments it takes. For a lambda, the count of arguments in its signature, or, where the signature is omitted, by the here highest-numbered of the three default argument names `x` (1), `y` (2) and `z` (3) used in the function definition, e.g. `{x+z}` has rank 3. Functions of rank 0, 1, 2 and 3 are called nullary, unary, binary and ternary respectively.

Of a **list**, the depth to which it is nested. A vector has rank 1.


## Reference, pass by

_Pass by reference_ means passing the name of an object (as a symbol atom) as an argument to a function, e.g. ``key `.q``.


## Right argument/s

See _Argument_


## Right-atomic function

A right-atomic function `f` is a binary that is atomic in its right, or second, argument. That is, for every valid left argument `x`, the unary function `f[x;]` is an atomic function.


## Right domain/s

See _Argument_


## Script

A script is a text file; its lines a list of expressions and/or system commands, to be executed in sequence. 
By convention, a script files has the extension `q`.

Within a script

-   function definitions may extend over multiple lines
-   an empty comment begins a _multiline comment_.


## Signature

The argument list that (optionally) begins a lambda, e.g. in `{[a;b](a*a)+(b*b)+2*a*b}`, the signature is  `[a;b]`.


## Simple table 

See [Table](#table).


## String

There is no string datatype in q. _String_ in q means a char vector, e.g. "abc". 


## Symbol

A symbol is an atom which holds a string of characters, much as an integer holds a string of digits. For example, `` `abc`` denotes a symbol atom. This method of forming symbols can only be used when the characters are those that can appear in names. To form symbols containing other characters, put the contents between double quotes, as in `` `$"abc-345"``.

A symbol is an atom, and as such has count 1; its count is not related to the number of characters that appear in its display. The individual characters in a symbol are not directly accessible, but symbols can be sorted and compared with other symbols. Symbols are analogous to integers and floating-point numbers, in that they are atoms but their displays may require more than one character. (If they are needed, the characters in a symbol can be accessed by converting it to a character string.)


## System command

Expressions beginning with `\` are [system commands](syscmds.md). (Or [multiline comments](syntax.md#multiline-expressions)).

```q
q)/ load the script in file my_app.q
q)\l my_app.q
```


## Table

A _simple table_ is a list of named lists of equal count.

```q
q)show t:([]names:`bob`carol`ted`alice; ages:42 39 51 44)
names ages
----------
bob   42
carol 39
ted   51
alice 44
```

It is also a list of dictionaries with the same keys.

```q
q)first t
names| `bob
ages | 42
```

Table syntax can declare one or more columns of a table as a _key_. The values of the key column/s of a table are unique.

```q
q)show kt:([names:`bob`carol`bob`alice;city:`NYC`CHI`SFO`SFO]; ages:42 39 51 44)
names city| ages
----------| ----
bob   NYC | 42
carol CHI | 39
bob   SFO | 51
alice SFO | 44
```

A _keyed table_ is a table of which one or more columns have been defined as its key. A table’s key/s (if any) are supposed to be distinct: updating the table with rows with existing keys overwrites the previous records with those keys. A table without keys is a simple table. 

A keyed table is a dictionary. Its key is a table.

```q
q)key kt
names city
----------
bob   NYC
carol CHI
bob   SFO
alice SFO
```


## Ternary

A map of rank 3, i.e. a function wiith three arguments; or a list of depth ≥3.


## Unary form

Most binary operators have unary forms that take a single argument. Q provides more legible covers for these functions.  

<i class="far fa-hand-point-right"></i> 
[Exposed infrastructure](exposed-infrastructure.md)


## Unary function

A map of rank 1, i.e. a function with 1 argument, or a list of depth ≥1.


## Unary operator

A primitive unary function that extends the application of its map argument, returning a derived function known as an _extension_. E.g. Over extends Add to return `+/`, an extension.

All the extenders except Compose are unary operators.

<i class="far fa-hand-point-right"></i>
Reference: [Extenders](../ref/extenders.md)


## Uniform function 

A uniform function `f` such that `count[x]~count f x`, e.g. `deltas`


## Uniform list

A list in which all items are of the same datatype. See also _vector_.


## Unsigned function

A lambda without a signature, e.g. `{x*x}`.  


## Value, pass by

_Pass by value_ means passing an object (not its name) as an argument to a function, e.g. `key .q`.


## Vector

A uniform list of basic types that has a special shorthand notation. A char vector is known as a _string_. 

## `x`

Default name of the first or only argument of an unsigned function.

## `y`

Default name of the second argument of an unsigned function.

## `z`

Default name of the third argument of an unsigned function.


## View

A view is a calculation that is re-evaluated only if the values of the underlying dependencies have changed since its last evaluation.
Views can help avoid expensive calculations by delaying propagation of change until a result is demanded.

The syntax for the definition is

```q
q)viewname::[expression;expression;…]expression
```

The act of defining a view does not trigger its evaluation.

A view should not have side-effects, i.e. should not update global variables.

<i class="far fa-hand-point-right"></i> 
[`view`, `views`](../ref/view.md)  
[`.Q.view`](../ref/dotq.md#qview-subview) (subview)
Tutorial: [Views tutorial](../tutorials/views)



