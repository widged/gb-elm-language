**Elm 0.??**, **copied, please ignore** 


## Functions

Elm's syntax for functions is very minimal, a name, soe arguments, an equals sign, and the body.

```elm
$ elm repl
> sum a b = a + b
> multiply a b = a*b
> isNegative n = n < 0
```

To call a function, give its name and its parameters, separated by spaces.

```elm
$ elm repl
> multiply 7 6 -- 42
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

Anonymous functions are defined with  `\arg1 arg2 -> expression`. The backslash is traditionally pronounced *lambda*, after the Greek letter used by programming language theorists, but you're welcome to say *function* if that helps you. It's usual not to have a space after the backslash, but we can.


Take the `filter` function. It filters through just those elements of a list that pass a specific test - i.e. those that return True from a specific function. We can pass it an anonymous function.

```elm
$ elm repl
> filterEven li = List.filter (\x -> x % 2 == 0) li
> filterEven [1, 2, 3, 4, 5]
[2,4] : List Int   
```

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

## Functions



#### Anonymous

Also known as _lambdas_
```elm
-- (\function arguments -> function body)
-- parenthesized, content starts with backslash
(\n -> n < 0)
(\x y -> x * y)
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### Infix
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

## ---------

Functions are usually prefix so from now on we won't explicitly state that a function is of the prefix form, we'll just assume it. In most imperative languages functions are called by writing the function name and then writing its parameters in parentheses, usually separated by commas. In Elm, functions are called by writing the function name, a space and then the parameters, separated by spaces. For a start, we'll try calling one of the most boring functions in Elm.

```elm
show identity 8 -- 8
```

The `identity` function takes a value and returns that value. As you can see, we just separate the function name from the parameter with a space. Calling a function with several parameters is also simple. The functions `min` and `max` take two things that can be put in an order (like numbers!). `min` returns the one that's lesser and `max` returns the one that's greater. See for yourself:

```elm
show min 9 10 -- 9
show min 3.4 3.2 -- 3.2
show max 100 101 -- 101
```

Function application (calling a function by putting a space after it and then typing out the parameters) has the highest precedence of them all. What that means for us is that these two statements are equivalent.

```elm
show identity 9 + max 5 4 + 1 -- 15
show (identity 9) + (max 5 4) + 1 -- 15
```

If a function takes two parameters, we can also call it as an infix function by surrounding it with backticks. For instance, the `rem` function takes two integers and does gives the remainder when you divide the first by the second. Doing `rem 92 10` results in a 2. But when we call it like that, there may be some confusion as to which number is doing the division and which one is being divided. So we can call it as an infix function by doing ``92 `rem` 10`` and suddenly it's much clearer.

```elm
show 92 `rem` 10 -- 2
```

Lots of people who come from imperative languages tend to stick to the notion that parentheses should denote function application. For example, in C, you use parentheses to call functions like `foo()`, `bar(1)` or `baz(3, "haha")`. Like we said, spaces are used for function application in Elm. So those functions in Elm would be `foo`, `bar 1` and `baz 3 "haha"`. So if you see something like `bar (bar 3)`, it doesn't mean that `bar` is called with `bar` and `3` as parameters. It means that we first call the function `bar` with `3` as the parameter to get some number and then we call `bar` again with that number. In C, that would be something like `bar(bar(3))`.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Baby's first functions
----------------------

In the previous section we got a basic feel for calling functions. Now let's try making our own! Go to our editor window and add this below the `import` line:

a function that takes a number and multiplies it by two.

```elm
doubleMe x = x + x
```

Functions are defined in a similar way that they are called. The function name is followed by parameters separated by spaces. But when defining functions, there's an `=` and after that we define what the function does. Now we can play with the function that we defined.

```elm
show doubleMe 9 -- 18
show doubleMe 8.3 -- 16.6
```

Because `+` works on integers as well as on floating-point numbers (anything that can be considered a number, really), our function also works on any number. Let's make a function that takes two numbers and multiplies each by two and then adds them together.

```elm
doubleUs x y = x*2 + y*2
```

Simple. We could have also defined it as `doubleUs x y = x + x + y + y`. Testing it out produces pretty predictable results.

```elm
show doubleUs 4 9 -- 26
show doubleUs 2.3 34.2 -- 73.0
show doubleUs 28 88 + doubleMe 123 -- 478
```

As expected, you can call your own functions from other functions that you made. With that in mind, we could redefine `doubleUs` like this:

```elm
doubleUs x y = doubleMe x + doubleMe y
```

This is a very simple example of a common pattern you will see throughout Elm. Making basic functions that are obviously correct and then combining them into more complex functions. This way you also avoid repetition. What if some mathematicians figured out that 2 is actually 3 and you had to change your program? You could just redefine `doubleMe` to be `x + x + x` and since `doubleUs` calls `doubleMe`, it would automatically work in this strange new world where 2 is 3.

Functions in Elm don't have to be in any particular order, so it doesn't matter if you define `doubleMe` first and then `doubleUs` or if you do it the other way around.

Now we're going to make a function that multiplies a number by 2 but only if that number is smaller than or equal to 100 because numbers bigger than 100 are big enough as it is!

```elm
doubleSmallNumber x = if x > 100 then x else x*2
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

Right here we introduced Elm's if statement. You're probably familiar with if statements from other languages. The difference between Elm's if statement and if statements in imperative languages is that the else part is mandatory in Elm. In imperative languages you can just skip a couple of steps if the condition isn't satisfied but in Elm every expression and function must return something. We could have also written that if statement in one line but I find this way more readable. Another thing about the if statement in Elm is that it is an *expression*. An expression is basically a piece of code that returns a value. `5` is an expression because it returns `5`, `4 + 8` is an expression, `x + y` is an expression because it returns the sum of `x` and `y`. Because the else is mandatory, an if statement will always return something and that's why it's an expression. If we wanted to add one to every number that's produced in our previous function, we could have written its body like this.

```elm
doubleSmallNumber' x = (if x > 100 then x else x*2) + 1
```

Had we omitted the parentheses, it would have added one only if `x` wasn't greater than 100. Note the ' at the end of the function name. That apostrophe doesn't have any special meaning in Elm's syntax. It's a valid character to use in a function name. We usually use ' to
denote a slightly modified version of a function or a variable. Because ' is a valid character in functions, we can make a function like this.

```elm
conanO'Brien = "It's a-me, Conan O'Brien!"
```

There are two noteworthy things here. The first is that in the function name we didn't capitalize Conan's name. That's because functions can't begin with uppercase letters. We'll see why a bit later. The second thing is that this function doesn't take any parameters. When a function doesn't take any parameters, we usually say it's a *definition* (or a *name*). Because we can't change what names (and functions) mean once we've defined them, conanO'Brien and the string "It's a-me, Conan O'Brien!" can be used interchangeably.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Function Composition

|Operator|Description|Type hint|
|--------|-----------|----------|
|`<\|`|backward (pipe) function application `f <\| x == f x`|`(a -> b) -> a -> b`
|`\|>`|forward (pipe) function application `x \|> f == f x`|`a -> (a -> b) -> b`
|`<<`|composes functions into one, arguments first applied to the function from the right side|`(b -> c) -> (a -> b) -> a -> c`
|`>>`|same as before except arguments first applied to the function from the left side|`(a -> b) -> (b -> c) -> a -> c`

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### `|>` as "Applying..." or "Piping to"

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

### `<<` as "composed with..."

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

### infix mode

Function calls happen before any infix operator. Parens indicate precedence.
```elm
cos (degrees 30) ^ 2 + sin (degrees 30) ^ 2 -- 1
```

First degrees is applied to 30, then the result is passed to the trig
functions, which is then squared, and the addition happens last.


### Recursive functions

Functions can be recursive.

```elm
fib n =
if n < 2 then 1
  else
    fib (n - 1) + fib (n - 2)
List.map fib [0..8] -- [1, 1, 2, 3, 5, 8, 13, 21, 34]
```

Another recursive function (use List.length in real code).
```elm
listLength aList =
  case aList of
    [] -> 0
    x::xs -> 1 + listLength xs
```


### Higher Order Functions

Pass functions as arguments to other functions.
```elm
double x = x * 2
List.map double [1..4] -- [2, 4, 6, 8]
List.map (\a -> a * 2) [1..4] -- [2, 4, 6, 8]
```


#### Currying

    concat : String -> String -> String
    concat a b = a ++ b

    listItem : String -> String
    listItem = concat "-> "


#### partial application

Partially apply a function by passing only some of its arguments. Then give that function a new name.

```elm
double = multiply 2
```

Or

```elm
-- Partial application
> minus x y = (-) x y
<function> : number -> number -> number
> minus1 = minus 1
<function> : number -> number
> minus1 11
-10 : number
```


#### other

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

### Everything is a function

Constants are a special case with no arguments.

```elm
answer = 42
```

* [Type constructors or types tags](11-type constructors)
* [Type aliases for records act as multi-parameter functions](05-record.md)



## Further Reading

* [learnyouanelm, higher order functions -- from learnyouanhaskell](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/06-higher-order-functions.md)
* [learnyouanelm, functionally solving problems](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/10-functionally-solving-problems.md)
* [learnyouanelm - Functors, Applicative Functors and Monoids](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/11-functors-applicative-functors-and-monoids.md)
