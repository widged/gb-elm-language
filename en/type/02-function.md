
**Elm 0.17**, **almost shareable** 


## Functions

Elm's syntax for functions is very minimal, a name, soe arguments, an equals sign, and the body.

```elm
$ elm repl
> sum a b = a + b
> multiply a b = a*b
> double x = x + x
> isNegative n = n < 0
```

To call a function, give its name and its parameters, separated by spaces.

```elm
$ elm repl
> min 9 10 -- min takes 2 numbers and returns the smallest one (minimum)
9 : number
> min 3.4 3.2
3.2 : Float
> max 100 101 -- min takes 2 numbers and returns the greatest one (maximum)
101 : number
> multiply a b = a*b
> multiply 7 6
42 : number
```

### Declaring functions

Functions can be declared in two ways.
* Named functions: `avgxy = (x+y)/2`
* Anonymous functions:`(\x y -> (x + y) / 2)`

#### named functions

We can define a function by naming it:

```elm
add : Int -> Int -> Int
add x y =  x + y

increment : Int -> Int
increment a = a + 1

isEven : Int -> Bool
isEven x = (x % 2 == 0)
```

#### anonymous functions

Anonymous functions are defined with  `\arg1 arg2 -> expression`. They are also known as _lambdas_ as he backslash is traditionally pronounced *lambda*, after the Greek letter used by programming language theorists. It's usual not to have a space after the backslash, but we can.

Take the `filter` function. It filters through just those elements of a list that pass a specific test - i.e. those that return True from a specific function. We can pass it an anonymous function.

```elm
$ elm repl
> filterEven li = List.filter (\x -> x % 2 == 0) li
> filterEven [1, 2, 3, 4, 5]
[2,4] : List Int   
```

```elm
-- (\function arguments -> function body)
-- parenthesized, content starts with backslash
(\n -> n < 0)
(\x y -> x * y)
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


#### named vs anonymous

Declaring a named function, and assigning an anonymous function to a named variable behave identically. We can define an anonymous function and then give it a name after.

```elm
$ elm repl
> add = \a b -> a + b -- instead of add a b = a + b
> add 2 4
6 : number
> concat : String -> String -> String
> concat = \a b -> a ++ b
> concat = \a -> (\b -> a ++ b)
> concat "elm" "o"
"elmo" : String
```

### Naming conventions

#### function'

The ' at the end of the function name. That apostrophe doesn't have any special meaning in Elm's syntax. It's a valid character to use in a function name. We usually use ' to
denote a slightly modified version of a function or a variable. 

Because ' is a valid character in functions, we can make a function like this.

```elm
conanO'Brien = "It's a-me, Conan O'Brien!"
```

#### must starts with lowercase letter

Functions cannot begin with uppercase letters. 


### Ok to use a function before it is declared

Functions in Elm don't have to be in any particular order. If you are calling `x = double 3`, it doesn't matter if the function `double` has been defined in the lines that precede or the the lines that follow.

### Everything is a function

Constants are a special case. When a function doesn't take any parameters, we usually say it's a *definition* (or a *name*). Because we can't change what names (and functions) mean once we've defined them, the name can be used in place of the value.

```elm
answer = 42
```

* [Type constructors or types tags](11-type constructors)
* [Type aliases for records act as multi-parameter functions](05-record.md)
* 
### Composing functions 

```elm
$ elm repl
> double x = x + x
> doubleUs x y = x*2 + y*2
> doubleUs' x y = doubleMe x + doubleMe y
```

Making basic functions that are obviously correct and then combining them into more complex functions. This way you also avoid repetition. What if some mathematicians figured out that 2 is actually 3 and you had to change your program? You could just redefine `doubleMe` to be `x + x + x` and since `doubleUs` calls `doubleMe`, it would automatically work in this strange new world where 2 is 3.

----

Like JavaScript, functions can take other functions as arguments. (We've already seen how currying lets them return functions.)

Let's look at a specialized version of the `List.map` function, which takes a function, and applies it to every element of a list of Float, returning a new list of Int as a result.

```elm
specialMap : (Float -> Int) -> List Float -> List Int
```

The first argument of this function needs to be a function, that takes a Float as a parameter and returns an Int. Whenyou read this annotation, it may help to say "Float goes to Int" a little bit faster, and then pause. Here, the brackets *do* matter. This is different than `Int -> Float -> List Int -> List Float`, which takes two numbers and a
list, but never a function.

We know that `round : Float -> Int`, so we can write:

```elm
roundMap : List Float -> List Int
roundMap = specialMap round
```

Even though `roundMap` doesn't take any arguments explicitly to the left of the equals, applying `specialMap` returns a function thanks to currying. We could also write `roundMap xs = specialMap round xs`; it's really a matter of style.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))

#### Operators

|Operator|Description|Type hint|
|--------|-----------|----------|
|`<\|`|backward (pipe) function application `f <\| x == f x`|`(a -> b) -> a -> b`
|`\|>`|forward (pipe) function application `x \|> f == f x`|`a -> (a -> b) -> b`
|`<<`|composes functions into one, arguments first applied to the function from the right side|`(b -> c) -> (a -> b) -> a -> c`
|`>>`|same as before except arguments first applied to the function from the left side|`(a -> b) -> (b -> c) -> a -> c`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### `|>` as "Applying..." or "Piping to"

The `|>` operator can be read as "applying...". It chains function calls by piping the result of a computation to the next function. 

```elm
$ elm repl
> import String
> "hello " ++ "elm! " |> String.toUpper |> String.repeat 2 |> String.trimRight
"HELLO ELM! HELLO ELM!" : String
```

Easier to read than 
`String.trimRight (String.repeat 2 (String.toUpper ("hello " ++ "elm! ")))`


Again, as spaces bind tightest of all the left hand argument of `|>` is effectively appended at the end. So `res4` is also "OnceUponATime"

```elm
res4 = "Time" |> concat4 "Once" "Upon" "A"
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

#### `<|` as "Applied to..."

The `<|` operator can be read as "applied to...". We can read the code here as "increment applied to 17":

```elm
res1 = increment <| 17    -- res1 is 18
```

Since spaces bind tightest of all, the right hand argument of `<|` is effectively appended at the end. So `res2` is "OnceUponATime"

```elm
res2 = concat4 "Once" "Upon" "A" <| "Time"
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

#### `<<` as "composed with..."

The `<<` operator can be read "composed with...".
It composes functions right to left. Although that might not
seem useful it reads very well, as demonstrated here:

```elm
fun5 : Float -> Bool
fun5 = not << isEven << sqrt
```

Now `fun5` is a function that does `not(isEven(sqrt(...)))`.

```elm
res6 = fun5 81    -- True, because the square root of 81 is indeed not even
res7 = fun5 64    -- False, because the square root of 64 is even
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))


#### `>>` as "and then..."

The `>>` operator can be read "and then...".
It composes functions left to right. It looks intuitive but
probably doesn't read so well.
This next function does the same as the one above.

```elm
fun8 : Float -> Bool
fun8 = sqrt >> isEven >> not

res9  = fun8 81    -- True, because the square root of 81 is indeed not even
res10 = fun8 64    -- False, because the square root of 64 is even
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

### infix vs prefix mode

Functions are usually prefix so from now on we won't explicitly state that a function is of the prefix form, we'll just assume it. 

Function calls happen before any infix operator. Parens indicate precedence.
```elm
cos (degrees 30) ^ 2 + sin (degrees 30) ^ 2 -- 1
```

First degrees is applied to 30, then the result is passed to the trig functions, which is then squared, and the addition happens last.

#### Infix

If a function takes two parameters, we can also call it as an infix function by surrounding it with backticks. For instance, the `rem` function takes two integers and does gives the remainder when you divide the first by the second. Doing `rem 92 10` results in a 2. But when we call it like that, there may be some confusion as to which number is doing the division and which one is being divided. So we can call it as an infix function by doing ``92 `rem` 10`` and suddenly it's much clearer.

```elm
show 92 `rem` 10 -- 2
```

-----

Functions that placed between two arguments and enclosed in backticks <code>`</code> are called _infix_
```elm
-- Normal
> min 1 2
1 : number

-- Infix
> 1 `min` 2
1 : number
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### Prefix

Functions that placed in front of arguments while enclosed in parentheses are called _prefix_

```elm
-- Normally you would do this
> "abcde" ++ "fghij"
"abcdefghij" : String

-- Prefix
> (++) "abcde" "fghij"
"abcdefghij" : String
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Higher Order and Recursive Functions

See [Thinking functionally](../thinking functionally/02-recursion)



## Further Reading

* [learnyouanelm, higher order functions -- from learnyouanhaskell](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/06-higher-order-functions.md)
* [learnyouanelm, functionally solving problems](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/10-functionally-solving-problems.md)
* [learnyouanelm - Functors, Applicative Functors and Monoids](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/11-functors-applicative-functors-and-monoids.md)
