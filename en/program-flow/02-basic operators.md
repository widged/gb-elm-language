**Elm 0.17**, **shareable**

## Operators

An operator is a special kind of function name that appears between its (two) arguments, such as the `+` in `x + y` or the `++` in `xs ++ ys`. Operator names begin with a symbol. Any (non-symbolic) function of two arguments can be converted into an operator by enclosing it in back quotes, and any operator can be converted to a prefix name by enclosing it in parentheses. For example,
* `3 + 4`	is the same as	`(+) 3 4`
* `div 3 4`	is the same as	<code>3 `div` 4</code>

Indeed, operators in Elm are actually _functions_. `+` is a function that takes two numbers and adds them. This is what we call an *infix* function. You can learn more about these in the chapter on [functions](../type/02-function.md).

### Arithmetic

Included in the Elm core library are the standard arithmetic operators +, -, *, and /. They work mostly as you’d expect.

|Operator|Description|Type signature|
|--------|-----------|----------|
|`-`|unary -|`<function> : number -> number`
|`+`|addition|`<function> : number -> number -> number`
|`-`|subtraction|`<function> : number -> number -> number`
|`*`|multiplication|`<function> : number -> number -> number`
|`/`|floating point division|`<function> : number -> number -> Float`
|`//`|integer division, discard the reminder|`<function> : Int -> Int -> Int`
|`rem`|division remainder|`<function:rem> : Int -> Int -> Int`
|`^`|exponentiation|`<function> : number -> number -> number`
|`%`|modulo|`<function> : Int -> Int -> Int`

Note that the integer division operator is written as `//`, and rounds down to the nearest integer if the result is a proper fraction. The symbol `-` can also be used as unary operator in expressions as `-12` (`+` cannot).

We can use the Elm REPL to try these operators.

```elm
$ elm repl
> -12
> -12.5
> +12       -- ERROR
> 2 + 15    -- 17
> 8 - 1     -- 7
> 49 * 100  -- 4900
> 1892 - 1472 -- 420
> 5 / 2     -- 2.5 (no need to first convert integers to floats)
> 5 // 2    -- 2
> 5.0 // 2  -- ERROR (Not an integer)
> 5 `rem` 2 -- 1 (note the back quotes, same as: rem 5 2)
> 5 ^ 2     -- 25 (exponent)
```

#### Precedence Rules

Operators have different levels of precedence (binding power). For arithmetic operators, mathematical convention is usually followed. Exponentiation has higher priority than multiplication, division, and remainder, which in turn have higher priority than addition and subtraction. For example,

```elm
> 3 * 4 + 2 -- means (3 * 4) + 2
> 3 * 4 ^ 2 -- means 3 * (4 ^ 2)
```
We can add parentheses to avoid any ambiguity.

```elm
$ elm repl
> (3 * 4) + 2 -- 14
> 3 * 4 + 2 -- 14
> 3 * (4 + 2) -- 18
```

#### Associativity Rules

Operators with the same level of precedence normally have an order of association, either to the left or right. Arithmetic operators generally associate to the left.

```elm
3 - 4 - 2 -- means (3 - 4) - 2
3 - 4 + 2 -- means (3 - 4) + 2
3 / 4 * 5 -- means (3 / 4) * 5
```

However,  exponentiation associates to the right

```elm
3 ^ 4 ^ 2	-- means	3 ^ (4 ^ 2)
```

If an operator is mathematically associative, such as `a + b + c`, the order doesn't matter. Associating to the left `(a + b) + c` or right `a + (b + c)` would produce the same result.


### Comparison Operators

Term comparisons take two expressions on either side of the comparison operator. The result of the expression is one of the Boolean value `True` or `False`. The values on both side must be of compatible types (`5 == 5` and `5 == 5.0` are valid comparisons, but `5 == "5"` will cause the compiler to complain that the second argument is not a number).

As well as comparisons for (in)equality, you can compare the ordering between values. The values must then be of comparable type. Comparisons are performed according to the natural ordering for each particular data type.


|Operator|Description|Type signature|
|--------|-----------|----------|
|`==`|equal|`<function> : a -> a -> Bool`
|`/=`|not equal|`<function> : a -> a -> Bool`
|`<`|less than|`<function> : comparable -> comparable -> Bool`
|`<=`|less than or equal|`<function> : comparable -> comparable -> Bool`
|`>`|greater than|`<function> : comparable -> comparable -> Bool`
|`>=`|greater than or equal|`<function> : comparable -> comparable -> Bool`
|`min`|minimum|`<function:min> : comparable -> comparable -> comparable`
|`max`|maximum|`<function:max> : comparable -> comparable -> comparable`

```elm
$ elm repl
> 5 == 5 -- True
> 1 == 0 -- False
> 5 /= 5 -- False
> 5 /= 4 -- True
> "hello" == "hello" -- True
> 'a' < "h" -- ERROR (incompatible types)
> 1 < 10 -- True
> min 5 6 -- 5
> min "a" "b" -- "a"
> max 5 6 -- 6
```

#### Comparing complex data types

You can also use comparison operators on structural data types whose elements are comparable, such as lists or tuples.

```elm
$ elm repl
> (1,2) < (3,4)
> [1,2] < [3,4]
> {name = "alice"} < {name = "anna"}
ERROR (record is not a comparable)
```

Lists are ordered lexicographically, like the words in a dictionary. The first elements are compared, and whichever is smaller indicates the smaller list: if they are the same, the second elements are compared, and so on. When one list is exhausted, that is the smaller list. So:

```elm
$ elm repl
> ["aa","zz"]<["ab","aa"]
True : Bool
> ["aa","bb"]<["aa","bb","cc"]
True : Bool
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
$ elm repl
> True && False -- False
> True && True -- True
> False || True -- True
> not False -- True
> not (True && True) -- False
```

### Joining operators

|Operator|Description|Type signature|
|--------|-----------|----------|
|`++`|join appendable values|`<function> : appendable -> appendable -> appendable`|
|`::`|add a value to the front of a list|`<function> : a -> List a -> List a`|

```elm
$ elm repl
> "a" ++ "b" -- "ab"
> [1] ++ [2] -- [1,2]
> 1 :: 2 :: [] -- [1,2]
```

Note that the list-forming operator (::) is defined to be right associative.

```elm
$ elm repl
> 1 :: 2 :: [] -- means 1 :: (2 :: [])
> (1 :: 2) :: [] -- ERROR
```

### Function operators

see [functions](../type/02-function.md)

### Precedence over operators

Functional application has the highest precedence and associates to the left.

```elm
$ elm repl
> f a + b -- means (f a) + b
> identity 9 + max 5 4 + 1 -- 15
> (identity 9) + (max 5 4) + 1 -- 15
```

In contrast, conditional expressions have very low binding power

```elm
$ elm repl
> let f x y = if x % 2 == 0 then x * y else x + y in f 2 4 -- 8
> let f x y = if x % 2 == 0 then (x * y) else (x + y) in f 2 4  -- 8
> let f x y = (if x % 2 == 0 then x * y else x) + y in f 2 4 -- 12
```

#### Declaring new operators

We can declare new operators; for example:

```elm
(+++) : Int -> Int -> Int
(+++) x y = if (x % 2) == 0 then y else x + y
```

### Further Reading

* [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md)
