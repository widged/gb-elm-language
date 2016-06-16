**Elm 0.??**, **copied, please ignore** 

# Primitive types

Common primitive types include `Int`, `Float`, `Bool`, and `String`. You can also pair up types into tuples, for example
`(Int, Bool)`. This expands to arbitrarily many elements, i.e. `(Int, Float, Int)` is a 3-tuple with first element `Int`,
second `Float`, third `Int`.

```elm
myTuple : (String, Int, Bool)
myTuple = ("the answer", 42, True)
```

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md]])

## Literals

The types of literals are built into the language.

```elm
True : Bool

42 : Int
6.28 : Float

"hello" : String
'x' : Char

["welcome", "to", "elm"] : List String
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md) and [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


--------------------------------------------------
## Booleans

`&&`,`||`, `not`

(source: ???)

~~~~ {.Elm:hs name="code"}
not True -- False
not False -- True
1 == 1 -- True
1 /= 1 -- False
1 < 10 -- True
~~~~

(source: ???)

### Bool

`Bool` is a boolean type. It can have only two values: `True` and `False`.

(source: ???)

### Booleans

```elm
> True
True : Bool
> False
False : Bool
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


--------------------------------------------------
## Numbers

* Arithmetic expressions: `+`,`-`, `*`, `/` (result is always a float), `//` (integer division), `%` (mod), parentheses
* Comparators: `<`, `<=`, `==`, `!=`, `>=`, `>`

### Numbers

Every number literal without a decimal point can be either an Int or a Float.

~~~~ {.Elm:hs name="code"}
33 / 2 -- 16.5 with floating point division
33 // 2 -- 16 with integer division
~~~~

(source: ???)

## Int

`Int` stands for integer. It's used for whole numbers. `7` can be an `Int` but
`7.2` cannot. `Int` is bounded, which means that it has a minimum and a
maximum value. The maximum possible value of `Int` is
`2147483647` and the `minimum` is `-2147483648`.

## Float

`Float` is a floating point with the same properties as JavaScript floats.

```elm
circumference : Float -> Float
circumference r =
    2 * pi * r
```

```elm
toPrint = circumference 4.0
25.132742
```

(source: ???)

## Numbers

Numeric types are `number` and `Float`, `number` represents both `Int` and `Float`:
```elm
> 1
1 : number
> 2.0
2 : Float
> truncate 0.1
0 : Int
> truncate 1
1 : Int
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


--------------------------------------------------
## Strings


## Char

`Char` represents a character. It's denoted by single quotes.

## String

`String` represents a string. It's denoted by double or triple quotes and
can contain many characters. Triple quoted strings are string literals
that can span across multiple lines, with line breaks and whitespace
being preserved in the string.

Tuples are types but they are dependent on their length as well as the
types of their components, so there is theoretically an infinite number
of tuple types, which is too many to cover in this tutorial. Note that
the empty tuple `()` is also a type which can only have a single value: `()`.
This value is read as `"unit"` and is the common way to denote an empty
value with no specific meaning.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))


## Operations on strings

* Strings: `"hello"`. Strings are _not_ lists of characters.
* Chars: `'a'`

`++` is string concatenation

(source: ???)


### Strings and characters

~~~~ {.Elm:hs name="code"}
"This is a string because it uses double quotes."
'a' -- characters in single quotes
~~~~

Strings can be appended.

~~~~ {.Elm:hs name="code"}
"Hello " ++ "world!" -- "Hello world!"
~~~~

(source: ???)

### Strings

String types are `char` and `String`

```elm
> 'a'
'a' : Char
> "Hello"
"Hello" : String
```

Multi-line string
```elm
"""
Hello
World
"""
```

Single quotes are for `char` only
```elm
> 'ab'
-- SYNTAX PROBLEM --
> "ab"
"ab" : String
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
