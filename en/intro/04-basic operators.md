# Operators

Elm operators are _functions_. For instance, `*` is a function that takes two numbers and multiplies them. As you've seen, we call it by sandwiching it between them. This is what we call an *infix* function. You can learn more about these in the chapter on [functions](../type/03-function.md).

## Overview

### Arithmetic
|Operator|Description|Type hint|
|--------|-----------|----------|
|`+`|addition|`number -> number -> number`
|`-`|subtraction|`number -> number -> number`
|`*`|multiplication|`number -> number -> number`
|`/`|floating point division|`Float -> Float -> Float`
|`//`|integer division, discard the reminder|`Int -> Int -> Int`
|`^`|exponentiation|`number -> number -> number`
|`%`|modulo|`Int -> Int -> Int`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

For instance, you can type any of these in the elm REPL. 

```bash
$ elm repl
> 2 + 15 -- 17
> 8 - 1 -- 7
> 49 * 100 -- 4900
> 1892 - 1472 -- 420
> 5 / 2 -- 2.5
> 5 ^ 2 -- 25 (exponent)
```

The usual precedence rules are obeyed. We can use parentheses to make the precedence explicit or to change it.

```bash
$ elm repl
> (50 * 100) - 4999 -- 1
> 50 * 100 - 4999 -- 1
> 50 * (100 - 4999) -- -244950
```

### Comparison
|Operator|Description|Type hint|
|--------|-----------|----------|
|`==`|equal|`comparable -> comparable -> Bool`
|`/=`|not equal|`comparable -> comparable -> Bool`
|`<`|less than|`comparable -> comparable -> Bool`
|`<=`|less than or equal|`comparable -> comparable -> Bool`
|`>`|greater than|`comparable -> comparable -> Bool`
|`>=`|greater than or equal|`comparable -> comparable -> Bool`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

```elm
5 == 5 -- True
1 == 0 -- False
5 /= 5 -- False
5 /= 4 -- True
"hello" == "hello" -- True
```


### Logical
|Operator|Description|Type hint|
|--------|-----------|----------|
|`&&`|logical and|`Bool -> Bool -> Bool`
|`||`|logical or|`Bool -> Bool -> Bool`
|`not`|logical not|`Bool`
|`xor`|biwise XOR|`Bool -> Bool -> Bool`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

```elm
True && False -- False
True && True -- True
False || True -- True
not False -- True
not (True && True) -- False
```

### Function Composition
|Operator|Description|Type hint|
|--------|-----------|----------|
|`<|`|backward (pipe) function application `f <| x == f x`|`(a -> b) -> a -> b`
|`|>`|forward (pipe) function application `x |> f == f x`|`a -> (a -> b) -> b`
|`<<`|composes functions into one, arguments first applied to the function from the right side|`(b -> c) -> (a -> b) -> a -> c`
|`>>`|same as before except arguments first applied to the function from the left side|`(a -> b) -> (b -> c) -> a -> c`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Other
|Operator|Description|Type hints|
|--------|-----------|----------|
|`++`|put appendable things together|`appendable -> appendable -> appendable`|
|`::`|add an element to the front of a list|`a -> List a -> List a`|
|`as`|keyword that creates aliases for values `(x, y) as t == t = (x, y)`|`a -> a`|

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))





