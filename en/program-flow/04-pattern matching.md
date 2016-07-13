**Elm 0.17**, **almost shareaable**

## Pattern matching (destructuring)

Before we start discussing branching and looping, you need to learn about the important underlying supporting mechanism: pattern matching. Pattern matching is fundamental to most modern functional languages. Pattern matching is a construct that attempts to match a right-side term to the left-side pattern.


Elm defines a fixed set of patterns for use in function definitions and case expressions. They take this general form:

```elm
let pattern = expression in result

myFunction pattern = expression

myFunction a b =
  case (a,b) of
    pattern1 -> expression1
    pattern2 -> expression2
```

Pattern matching consists of specifying patterns to which some data should conform and then checking to see if it does and deconstructing the data according to those patterns. In the process, variables from the pattern are bound to corresponding subterms from the term.

```elm
let (name, age) = ("Bob", 25) in "Hello " ++ name
```

Let’s break down what happens here: The expression on the right side, `("Bob", 25)`, is evaluated . The resulting value is matched against the left-side `(name, age)` pattern. Whenever a variable name exists in the left-side pattern, it is bound to the corresponding right-side term. The result is evaluated, with possible references to variables in the pattern

If a term doesn’t match the pattern, an error is raised.

```elm
> let (name, age, address) = ("Bob", 25) in "Hello " ++ name
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
```

### Wildcard match (_)

Occasionally there are values that we really care nothing about. We are still required to match against them. In such situations, <code>_</code> can be used as a wilcard match. Each wildcard will independently match anything, but in contrast to other named variables, each will bind nothing; for this reason more than one are allowed in an equation.

```elm
> let (name,age,_,_) = ("Bob", 25, "30 Sunrise drive", "San Francisco") in name ++ ", " ++ (toString age)
"Bob, 25" : String
```

### Matching function arguments

Function arguments are patterns. When you call a function, the arguments you provide are matched against the patterns specified in the function definition.

```elm
> add (a,b) = a + b
> add (1, 2)
3 : number
```

The term being matched is the argument provided to the function call. The pattern you match against is the argument specifier, in this case (a, b). In the code example, the function expects a two-element tuple and binds the tuple’s elements to variables a and b. Of course, if you provide anything that isn’t a two-element tuple, an error will be raised:

### Case Expressions

In Case expressions, a sequence of syntactic expressions called patterns is used to choose between a sequence of results of the same type. If the first pattern is matched, then the first result is chosen; otherwise, if the second is matched, then the second result is chosen, and so on.

```elm
import Html

main = Html.text (toString (isEmpty [], isEmpty [1]))
-- (True,False)

isEmpty : List a -> Bool
isEmpty xs =
    case xs of
      [] -> True
      _  -> False
```

Matching against equations is in order from top to bottom.  If a match succeeds, some parts of the term are bound to variables from the pattern. The result of the expression is returned. If the match fails, an error is raised.

```elm
case xs of
  _  -> False
  [] -> True
==================================== ERRORS ====================================
-- REDUNDANT PATTERN -----------------------------------------------------------
The following pattern is redundant. Remove it.
11|       [] -> True
Any value with this shape will be handled by a previous pattern.
```

Exact values can be used at any point. This works with String, Char, etc. and any Tuple/List/union type built up of them.

```elm
import Html

divtext a = Html.div [] [Html.text (toString a)]

main = Html.div []
  [ divtext (isThree 3)
  , divtext (isThree 4)
  ]
{-
  "three"
  "not three"
-}

isThree : number -> String
isThree x =
  case x of
    3 -> "three"
    _ -> "not three"

```

### Nesting

Nesting of patterns is permitted (to arbitrary depth).

```elm
> let (_, (a, (b, _))) = (1, (2,(3,4))) in a
2 : number
```

### Destructuring Complex Variables

Pattern matching is a feature that makes manipulations with complex variables (such as tuples and lists) a lot easier.

#### on a Tuple

A tuple pattern matches any tuple of the same arity whose components all match the corresponding patterns in order.

It is useful when you call a function that returns a tuple and you want to bind individual elements of that tuple to separate variables, here width and height.

```elm
> area (width, height) = width * height
> dim = (6, 7)
> area dim
42 : number
```

##### wildcard

As in any pattern, the wilcard can be used to signal that we don't care about binding a specific value to a variable.

For example, in the Elm Basics package, the [fst and snd functions](https://github.com/elm-lang/core/blob/master/src/Basics.elm#L596) are defined as follows

```elm
{-| Given a 2-tuple, returns the first value. -}
fst : (a,b) -> a
fst (a,_) = a

{-| Given a 2-tuple, returns the second value. -}
snd : (a,b) -> b
snd (_,b) = b
```


##### different variable names

Different variable names must be used.

```elm
> let (a,a,a) = (1,1,1) in a
-- DUPLICATE DEFINITION -------------------------------------- repl-temp-000.elm

There are multiple values named `a` in this let-expression.
```

This obviously applies to when multiple tuples are specified as arguments. For instance, take a function that takes two vectors in a 2D space (that are in the form of pairs) and adds them together? To add together two vectors, we add their x components separately and then their y components separately.

```elm
import Html
main = Html.text (toString (addVectors (1,2) (3,4)))
-- (4,6)
addVectors : (Float, Float) -> (Float, Float) -> (Float, Float)
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)
```

#### on a List

A list pattern matches any list of the same length whose elements all match the corresponding patterns in order.

```elm
import Html

main = Html.text (toString
  ( testChar ['a', 'b', 'c']
  , testChar ['z', 'y', 'x']
  ))
-- (True,False)

testChar : List Char -> Bool
testChar lst =
   case lst of
     ['a', _, _] -> True
     _           -> False
```

This function will assess if a list contains precisely three characters beginning with ’a’. Obviously, having to a named variable or wildcard for each item of the list is not very practical.

Therefore, patterns take advantage of the fact that `[1,2,3]` is syntactic sugar for `1::2::3::[]`. The cons operator (`::`) can also be used to construct patterns, which match any non-empty list whose first and remaining elements match the corresponding patterns in order.

You can match with the empty list `[]` or any pattern that involves `::` and the empty list. The pattern, `[x]` is used to capture a list with only one element. The pattern `x::xs` is used to bind the head of the list to `x` and the rest of it to `xs`.

In Elm, many list manipulation functions are written in native format. If we were to implement them the functional way, we could have:

```elm
import Html
import Debug

main = Html.text (toString
  ( null []
  , null ['a', 'b', 'c']
  , head ['a', 'b', 'c']
  , tail ['a', 'b', 'c']
  ))
{-
  ( True
  , False
  ,'a'
  ,['b','c']
  )
-}

null : List a -> Bool
null lst =
  case lst of
    [] -> True
    _::_ -> False

head : List a -> a
head lst =
  case lst of
    [] -> Debug.crash "head called on an empty list"
    x::_ -> x

tail : List a -> List a
tail lst =
  case lst of
    [] -> Debug.crash "tail called on an empty list"
    _::xs -> xs
```
Note. `Debug.crash` reflects the need to provide patterns for every possible branching. This is why the Elm library returns a Maybe type.

Here are a few other common patterns for lists

```elm
[]       -- matches an empty list
[x]      -- matches a list with exactly one element, and bind that element to the variable x
[x,y]    -- matches against a list of  exactly two elements.
x::[]    -- equivalent to `x`
x::y::[] -- equivalent to `[x,y]`
x::xs    -- matches a list of at least one item, and bind the head element to the variable x. As `x:xs` shows, it is customary to write a letter or a small word followed by the same identifier in plural.
x::y::z::zs -- matches against lists that have three elements or more; the first three elements will be bound to variables and the rest of the list to zs.
x::_     -- matches the "head" of lists of length 1 or more and bind it to a `x` variable. The anonymous variable _ is used as placeholder for the tail.
_::xs    -- matches the "tail" of lists of length 1 or more and bind it to a `xs` variable. The anonymous variable _ is used as placeholder for the head.
x::y::_  -- matches lists of length 2 or more; the first two elements are bound to variables
x ++ y -- Compiler errror. There is no way to establish how a list should be split into two lists, so `++` cannot be used in pattern matches.
```

Matching lists is more often done by relying on their recursive nature. The following steps are followed:

1. Define the result for any known input. This is typically the empty list. This is also known as the edge condition. For instance, the length of an empty list is 0, the sum of the elements of an empty list is 0.
2. Split the list into a head and a tail. Combine the head with a recursive call over the tail. For instance, once the head is removed, the length of a list is 1 + the length of the tail.

```elm
import Html

divtext a = Html.div [] [Html.text (toString a)]

main = Html.div []
  [ divtext (member 3 [1,2,3,4], member 5 [1,2,3,4])
  , divtext (sum [], sum [1], sum [1,2], sum [1,2,3,4,5])
  , divtext (length [], length [1], length [1,2], length [1,2,3,4,5])
  ]
{-
(True,False)
(0,1,3,15)
(0,1,2,5)
-}

length : List a -> Int
length list =
    case list of
        [] -> 0
        (_::xs) -> 1 + length xs
-- The length of ['h','a','m'] is `1 + (1 + (1 + 0))`

sum : List number -> number
sum list =
    case list of
        [] -> 0
        (x::xs) -> x + sum xs
-- The sum of [1,2,3] is `1 + (2 + (3 + 0))`

{-
Checks if a value is present in a list, using
-}
member : a -> List a -> Bool
member value list =
  case list of
    [] -> False
    (x::xs) ->
      if x == value then
        True
      else
        member value xs
```

##### wildcard variable (_)

Use the anonymous variable <code>_</code>  to match any term that you don't actually care about.

##### cover all possible patterns

Because a list length is not fixed, it is not possible to use a `[x,y]` pattern, the way we would use `(x,y)` for a tupple on its own. This is because the pattern is not safe. The compiler will alert you to missing or extra cases.


```elm
> let [a, b] = [12, 25] in a
==================================== ERRORS ====================================

-- PARTIAL PATTERN ------------------------------------------- repl-temp-000.elm

The pattern used here does not cover all possible values.
```

##### be mindful of order

If we put `[x]` after `x::xs` last, it would never match because `x::xs` matches any list of length of 1 or more.


#### on a record

```elm
> let {name} = {name = "Bob"} in name
"Bob" : String
```
When matching a map, the left-side pattern doesn’t need to contain all the keys from the right-side term:

```elm
> let {name} = {name = "Bob", age = 25} in name
"Bob" : String
```

Of course, a match will fail if the pattern contains a key that’s not in the matched term:

```elm
> let {name, address} = {name = "Bob", age = 25} in name
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
This record is being used in an unexpected way.
```

Patterns can be nested:

```elm
> let ({name}, _) = ({name = "Bob", age = 25}, "ignored") in name
"Bob" : String
```

```elm
-- Requires the argument with x and y fields
multiply {x,y} = x * y
```

Use curly brackets to pattern match record field names. Use let to define intermediate values.

```elm
volume {width, height, depth} =
  let
    area = width * height
  in
    area * depth
volume { width = 3, height = 2, depth = 7 } -- 42
```

### on a Maybe

```elm
case List.head aList of
  Just x -> "The head is " ++ toString x
  Nothing -> "The list was empty."
```

```elm
f : Maybe Int -> String
f n =
  case n of
    Just 42 -> "You got it!"
    Just 41 -> "Almost!"
    Just _ -> "Nope!"
    Nothing -> "You have to at least try!"
The full value that is matched can be bound with the as keyword:

f : (Int, Int) -> String
f point =
  case point of
    (0, _) as thePoint -> toString thePoint ++ " is on the x axis"
    _ as thePoint-> toString thePoint ++ " is not on the x axis"
```

(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

### Matching on Union Types

```elm
import Html

divtext a = Html.div [] [Html.text (toString a)]

main = Html.div []
  [ divtext (tellType (IsInt 2))
  , divtext (tellType (IsString "a"))
  , divtext (tellType (IsSubtype (Sub1 1)))
  , divtext (tellType (IsSubtype (Sub2 "b")))
  ]
{-
  "int"
  "string"
  "int"
  "string"
-}

type Subtypes = Sub1 Int | Sub2 String
type Types = IsInt Int | IsString String | IsSubtype Subtypes  | IsOther

tellType : Types -> String
tellType x =
  case x of
    IsInt a -> "int"
    IsString a -> "string"
    IsSubtype (Sub1 a) -> "int"
    IsSubtype (Sub2 a) -> "string"
    _ -> "other"
```

### Further reading

* [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md)
* [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e)
