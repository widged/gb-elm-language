# Operators

Elm operators are _functions_.

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

We can also use several operators on one line and all the usual precedence rules are obeyed. We can use parentheses to make the precedence explicit or to change it.

```bash
$ elm repl
> (50 * 100) - 4999 -- 1
> 50 * 100 - 4999 -- 1
> 50 * (100 - 4999) -- -244950
```

### Bitwise
|Operator|Description|Type hint|
|--------|-----------|----------|
|`and`|bitwise AND|`Int -> Int -> Int`
|`or`|bitwise OR|`Int -> Int -> Int`
|`xor`|biwise XOR|`Int -> Int -> Int`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

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

### Logical
|Operator|Description|Type hint|
|--------|-----------|----------|
|`&&`|logical and|`Bool -> Bool -> Bool`
|`||`|logical or|`Bool -> Bool -> Bool`
|`not`|logical not|`Bool`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

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

## basic arithmetic


Boolean algebra is also pretty straightforward. As you probably know, `&&` means a boolean *and*, `||` means a boolean *or*. `not` negates a `True` or a `False`.


```elm
True && False -- False
True && True -- True
False || True -- True
not False -- True
not (True && True) -- False
```

Testing for equality is done like so.

```elm
5 == 5 -- True
1 == 0 -- False
5 /= 5 -- False
5 /= 4 -- True
"hello" == "hello" -- True
```

What about doing `5 + "llama"` or `5 == True`? Well, if we try the first
snippet, we get a nice error message!

    The right argument of (+) is causing a type mismatch.
    3| show 5 + "llama"

    As I infer the type of values flowing through your program, I see a conflict
    between these two types:

        number

        String

Long, but informative!

What Elm is telling us here is that `"llama"` is not a number and so it doesn't know how to add it to 5. Even if it wasn't `"llama"` but `"four"` or `"4"`, Elm still wouldn't consider it to be a number. `+` expects its left and right side to be numbers. If we tried to do `True == 5`, Elm would tell us that the types don't match. Whereas `+` works only on things that are considered numbers, `==` works on any two things that can be compared. But the catch is that they both have to be the same type of thing. You can't compare apples and oranges. We'll take a closer look at types a bit later. Note: you can do `5 + 4.0` because `5` is sneaky and can act like an integer or a floating-point number. `4.0` can't act like an integer, so `5` is the one that has to adapt.

You may not have known it but we've been using functions now all along. For instance, `*` is a function that takes two numbers and multiplies them. As you've seen, we call it by sandwiching it between them. This is what we call an *infix* function. Most functions that aren't used with numbers are *prefix* functions. Let's take a look at them.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))



