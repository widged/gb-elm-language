**Elm 0.??**, **copied, please ignore** 

# Primitive types

Primitive types are built into the language. They include `Int`, `Float`, `Bool`, and `String`. 

```elm
True : Bool

42 : Int
6.28 : Float

"hello" : String
'x' : Char

["welcome", "to", "elm"] : List String
```

## Boolean Types

### Bool

`Bool` is a boolean type. It can have only two values: `True` and `False`.

```elm
> True
True : Bool
> False
False : Bool
```

## Numeric Types

Numeric types are `Int` and `Float`. 

```elm
> 2.0
2 : Float
> truncate 0.1
0 : Int
> truncate 1
1 : Int
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Int

`Int` stands for integer. It's used for whole numbers. `7` can be an `Int` but `7.2` cannot. `Int` is bounded, which means that it has a minimum and a maximum value. The maximum possible value of `Int` is `2147483647` and the `minimum` is `-2147483648`.

## Float

`Float` is a floating point.

```elm
circumference : Float -> Float
circumference r =
    2 * pi * r
toPrint = circumference 4.0
25.132742
```

(source: ???)


--------------------------------------------------
## Text Strings

String types are `Char` and `String`

```elm
> 'a'
'a' : Char
> "Hello"
"Hello" : String
```


### Char

`Char` represents a character. It's denoted by single quotes `'a'`.

### String

`String` represents a string. It's denoted by double or triple quotes and can contain many characters. Strings are _not_ lists of characters.


Triple quoted strings are string literals that can span across multiple lines, with line breaks and whitespace being preserved in the string.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))





## Operations on strings


```elm
"This is a string because it uses double quotes."
'a' -- characters in single quotes
```

Strings can be appended with `++`.

```elm
"Hello " ++ "world!" -- "Hello world!"
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

