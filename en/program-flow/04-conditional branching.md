**Elm 0.17**, **shareable**

## Conditional Branching

In this chapter, we provide a detailed treatment of conditional and branching constructs in Elm. They work significantly differently than what you may have been used to. One thing to keep in mind when branching is that everything in Elm is an expression that has a return value. This includes constructs like "if ... then" and "case ... of". All branching options must be covered.

Elm provides two constructs for conditional branching:
 * `if ... then ... else ...`
 * `case ... of ...`

### If ... then ... else ...

Both _then_ and _else_ expressions must be provided. If the condition is met, then the return value is the result of the executed _then_ block and the executed _else_ block otherwise.


```elm
if condition then value_if_True else value_if_False
if | condition -> expression | condition -> expression ...
| otherwise -> expression
case expression of pattern -> expression ...
pattern -> expression
```

If statement are *expressions*. They must return a value. This means that the _else_ part is mandatory and that the _then_ and _else_ branches must be of the same type.

```elm
if powerLevel > 9000 then
  "WHOA!"
else
  "meh"
```

_If_ statements can be chained. All the branches of an if need to match so that no matter which one we take, we get back the same type of value overall.

```elm
if n < 0 then
  "n is negative"
else if n > 0 then
  "n is positive"
else
  "n is zero"
```

The condition must evaluate to True or False.

```elm
> if 1 then "nope" else "nope again"
- TYPE MISMATCH --
```


### Case ... of ...

The general syntax of case is as follows:

```elm
case expression of
  pattern_1 ->
    ...
  pattern_2 ->
    ...
...
```

The term pattern here indicates that _case_ tries to match the expression against patterns defined after the `of` keyword. The first pattern that matches the expression is executed, and the result of the corresponding block is the result of the entire case statement.


```elm
type User = Activated | Deleted

update state =
  case state of
    Activated ->
      -- do something
    Deleted ->
      -- do again
```

In case of passing tags with additional properties, parameters are passed along with type checking

```elm
type User
    = Activated Int
    | Deleted (Int, String)

update state =
  case state of
    Activated value ->
      -- do something with value
    Deleted values ->
      -- do something with values
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

Let's make a really trivial function that checks if the number we supplied to it is a seven or not.

```elm
lucky : number -> String
lucky x =
    case x of
        7 -> "LUCKY NUMBER SEVEN!"
        _ -> "Sorry, you're out of luck, pal!"
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

When you call lucky, the patterns will be checked from top to bottom and when it conforms to a pattern, the corresponding function body will be used. The only way a number can conform to the first pattern here is if it is 7. If it's not, it falls through to the second pattern, which matches anything and discards it. The _ is a wildcard that's used when we don't care what the specific value is. If you do care what the value is, you can write any variable name in this place and the value will be bound to that variable, like this:

```elm
lucky : number -> String
lucky x =
    case x of
        7 -> "LUCKY NUMBER SEVEN!"
        n -> "Sorry, you're out of luck, pal! You had " ++ toString n
```

This function could have also been implemented by using an if statement. But what if we wanted a function that says the numbers from 1 to 5 and says "Not between 1 and 5" for any other number? Without pattern matching, we'd have to make a pretty convoluted if then else tree. However, with it:

```elm
sayMe : number -> String
sayMe x =
    case x of
        1 -> "One!"
        2 -> "Two!"
        3 -> "Three!"
        4 -> "Four!"
        5 -> "Five!"
        _ -> "Not between 1 and 5"
```

Note that if we moved the last pattern (the catch-all one) to the top, it would always say "Not between 1 and 5", because it would catch all the numbers and they wouldn't have a chance to fall through and be checked for any other patterns.

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

We could use pattern matching to define a factorial function *recursively*, the way it is usually defined in mathematics. We start by saying that the factorial of 0 is 1. Then we state that the factorial of any positive integer is that integer multiplied by the factorial of its predecessor. Here's how that looks like translated in Elm terms.

```elm
factorial : number -> number
factorial n =
    case n of
        0 -> 1
        _ -> n * factorial (n - 1)
```

This is the first time we've defined a function recursively. Recursion is important in functional languages and we'll take a closer look at it later. But in a nutshell, this is what happens if we try to get the factorial of, say, 3. It tries to compute 3 \* factorial 2. The factorial of 2 is 2 \* factorial 1, so for now we have 3 \* (2 \* factorial 1). factorial 1 is 1 \* factorial 0, so we have 3 \* (2 \* (1 \* factorial 0)). Now here comes the trick — we've defined the factorial of 0 to be just 1 and because it encounters that pattern before the catch-all one, it just returns 1. So the final result is equivalent to 3 \* (2 \* (1 \* 1)). Had we written the second pattern on top of the first one, it would catch all numbers, including 0 and our calculation would never terminate. That's why order is important when specifying patterns and it's always best to specify the most specific ones first and then the more general ones later. Also, once a pattern has matched, it completely stops trying other alternatives. 

Pattern matching must be complete. If we define a function like this:

```elm
charName : Char -> String
charName c =
    case c of
        'a' -> "Albert"
        'b' -> "Broseph"
        'c' -> "Cecil"
```

the compiler will notice that we haven't accounted for all possibilites, either by specifying a pattern for every possible value of c, or by including a wildcard that matches on every value. It will say:

```
This `case` does not have branches for all possibilities.
3|     case c of
4|         'a' -> "Albert"
5|         'b' -> "Broseph"
6|         'c' -> "Cecil"

You need to account for the following values:

    <values besides 'a', 'b', and 'c'>

Add a branch to cover this pattern!
```

It complains that we have missing patterns, and rightfully so. When making patterns, we should always include a catch-all pattern so that our program doesn't crash if we get some unexpected input. Features like this are why Elm can all but guarantee no runtime errors in your code.

As you can see, the syntax for case expressions is pretty simple:

```elm
case expression of
    pattern -> result
    pattern -> result
    pattern -> result
    ...
```

expression is matched against the patterns. The pattern matching action is the same as expected: the first pattern that matches the expression is used. If it falls through the whole case expression and no suitable pattern is found, a compilation error occurs.

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))




### Further Reading

* [elm-lang style guide](http://elm-lang.org/docs/style-guide)
* [elm style guide @NodeRedInk](https://github.com/NoRedInk/elm-style-guide)
