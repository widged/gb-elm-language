**Elm 0.17**, **copied, please ignore**

## Pattern matching (destructuring)

A match expression has this general form:

```elm
let pattern = expression in result

myFunction pattern = expression

myFunction a b =
  case (a,b) of
    pattern1 -> expression1
    pattern2 -> expression2
```

or

```elm
> let a = 1 + 3 in a
4 : number
```

Pattern matching consists of specifying patterns to which some data should conform and then checking to see if it does and deconstructing the data according to those patterns.

Matching against equations is in order from top to bottom.  If a match succeeds, some parts of the term are bound to variables from the pattern. The result of the expression is returned. If the match fails, an error is raised.

### Basic Usage

```elm
let (name, age) = ("Bob", 25) in "Hello " ++ name
```

In the example, the variables name and age are matched onto the tuple ("Bob", 25). Whenever a variable name exists in the left-side pattern, it always matches the corresponding right-side term. The variable is also bound to the term it matches.

### Matching function arguments

Function arguments are patterns. Calling a function tries to match the provided values to the patterns specified in the function definition.

```elm
> sum (a,b) = a + b
<function> : ( number, number ) -> number
> sum (1, 2)
3 : number
```

When you call a function, the arguments you provide are matched against the patterns specified in the function definition. The function expects a two-element tuple and binds the tuple’s elements to variables a and b.

When calling functions, the term being matched is the argument provided to the function call. The pattern you match against is the argument specifier, in this case (a, b).

Of course, if you provide anything that isn’t a two-element tuple, an error will be raised:


### Wildcard match (_)

Occasionally we aren’t interested in a value, but we still need to match on it. In such cases, <code>_</code> is used as a wilcard match. The anonymous variable works just like a named variable: it matches any right-side term. But no binding is created. This means you can match to multiple <code>_</code> in a single pattern.

```elm
let
  (a,b,_) = myTuple
in
  a ++ b
-- "AB" : String
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

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

### Complex Variables

Pattern matching is a feature that makes manipulations with complex variables (such as tuples and lists) a lot easier.

#### on a Tuple

Basic pattern matching of tuples:

```elm
> area (width, height) = width * height
> dim = (6, 7)
> area dim
42 : number
```

When the expression is evaluated, the variables width and height are bound to the corresponding elements of the tuple. This feature is useful when you call a function that returns a tuple and you want to bind individual elements of that tuple to separate variables.

```elm
> sum (a, b) = a + b
> let (a,b,c) = (1,2,3) in (a+b+c) * 100
600 : number
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

Pattern matching can also be used in the declared parameters of functions, and on tuples. What if we wanted to make a function that takes two vectors in a 2D space (that are in the form of pairs) and adds them together? To add together two vectors, we add their x components separately and then their y components separately. Here's how we would have done it if we didn't know about pattern matching:

```elm
addVectors : (Float, Float) -> (Float, Float) -> (Float, Float)
addVectors a b =
    (fst a + fst b, snd a + snd b)
```

Well, that works, but there's a better way to do it. Let's modify the function so that it uses pattern matching.

```elm
addVectors : (Float, Float) -> (Float, Float) -> (Float, Float)
addVectors (x1, y1) (x2, y2) =
    (x1 + x2, y1 + y2)
```

There we go! Much better. Note that this is already a catch-all pattern. The type of addVectors (in both cases) is addVectors : (Float, Float) -\> (Float, Float) - \> (Float, Float), so we are guaranteed to get two pairs as parameters.

fst and snd extract the components of pairs. But what about triples? Well, there are no provided functions that do that but we can make our own.

```elm
first : (a, b, c) -> a
first (x, _, _) = x

second : (a, b, c) -> b
second (_, y, _) = y

third : (a, b, c) -> c
third (_, _, z) = z
```

As a reminder, the \_ means that we really don't care what that part is,
so we just write a \_.

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))


#### nesting

Patterns can be arbitrarily nested.

```elm
let (_, (a,_)) = (1, (2,3)) in a
```

#### different variables

Different variables must be used.

```elm
> let (a,a,a) = (1,1,1) in a
-- DUPLICATE DEFINITION -------------------------------------- repl-temp-000.elm

There are multiple values named `a` in this let-expression.
```

#### on a List

Matching lists is more often done by relying on their recursive nature.

Let's make a trivial function that tells us some of the first elements of the list.

```elm
open "http://elm-lang.org/try"
import Html

main = Html.text (toString (tell [], tell [1], tell [1,2], tell [1,2,3,4,5]))
{- ("The list is empty","The list has exactly one element: 1","The list of at least one item whose head is 1","The list of at least one item whose head is 1") -}

tell : List a -> String
tell list =
  case list of
    [] -> "The list is empty"
    [x] -> "The list has exactly one element: " ++ toString x
    x::xs -> "The list of at least one item whose head is " ++ toString x
```

You can match with the empty list `[]` or any pattern that involves `::` and the empty list. The pattern, `[x]` is used to capture a list with only one element. The pattern `x::xs` is used to bind the head of the list to `x` and the rest of it to `xs`. It reflects the fact that `[1,2,3]` is syntactic sugar for `1::2::3::[]`. Pattern matches go in order. If we put [x] last, it would never match because `x::xs` also matches (xs would be the empty list).

This function is safe because it takes care of the empty list, a singleton list, a list with two elements and a list with more than two elements.

Note. It’s customary to write pattern matching on lists using a letter or a small word followed by the same identifier in plural, like `x:xs`. Note also that `++` cannot be used in pattern matches.

If you need only one element of the (head, tail) pair, you can use the anonymous variable <code>_</code>. Here are a few other examples.

```haskell
x::y::[] -- matches against a list of  exactly two elements
[x,y] -- also matches against a list of  exactly two elements
x::y::z::zs -- matches against lists that have three elements or more; the first three elements will be bound to variables and the rest of the list to zs.
x::y::_ -- matches lists of length 2 or more; the first two elements are bound to variables
```

Now that we know how to pattern match against list, let's make our own implementation of the `List.member` function, which checks if a value is present in a list, using

```elm
open "http://elm-lang.org/try"
import Html

main = Html.text (toString (member 1 [1,2,3,4], member 1 [2,3,4,5]))
-- (True,False)

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

We can also implement our own `List.length` function:

```elm
open "http://elm-lang.org/try"
import Html

main = Html.text (toString (length [], length [1], length [1,2], length [1,2,3,4,5]))
-- (0,1,2,5)

length : List a -> Int
length list =
    case list of
        [] -> 0
        (_::xs) -> 1 + length xs
```

This is similar to the factorial and member functions we wrote earlier. First we defined the result of a known input — the empty list. This is also known as the edge condition. Then in the second pattern we take the list apart by splitting it into a head and a tail. We say that the length is equal to 1 plus the length of the tail. We use <code>_</code> to match the head because we don't actually care what it is. Also note that we've taken care of all possible patterns of a list. The first pattern matches an empty list and the second one matches anything that isn't an empty list.

Let's see what happens if we call length' on ['h','a','m']. First, it will check if it's an empty list. Because it isn't, it falls through to the second pattern. It matches on the second pattern and there it says that the length is 1 + length' ['a','m'], because we broke it into a head and a tail and discarded the head. O-kay. The length' of ['a','m'] is, similarly, 1 + length' ['m']. So right now we have 1 + (1 + length' ['m']). length' ['m'] is 1 + length' []. And we've defined length' [] to be 0. So in the end we have 1 + (1 + (1 + 0)).

Let's implement `List.sum`. We know that the sum of an empty list is 0. We write that down as a pattern. And we also know that the sum of a list is the head plus the sum of the rest of the list. So if we write that down, we get:

```elm
import Html

main = Html.text (toString (sum [], sum [1], sum [1,2], sum [1,2,3,4,5]))
-- (0,1,2,5)

sum : List number -> number
sum list =
    case list of
        [] -> 0
        (x::xs) -> x + sum xs
```


##### limitations

List matching doesn't work similarly to tuples.

```elm
> let [a, b] = [12, 25] in a
==================================== ERRORS ====================================

-- PARTIAL PATTERN ------------------------------------------- repl-temp-000.elm

The pattern used here does not cover all possible values.
```


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

### Further reading

* [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md)
