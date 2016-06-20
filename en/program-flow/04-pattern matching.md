**Elm 0.17**, **copied, please ignore** 

## Pattern matching (destructuring)

We can evaluate expressions with pattern matching. Pattern matching consists of specifying patterns to which some data should conform and then checking to see if it does and deconstructing the data according to those patterns. 

Exact values of comparables can be used to match when destructuring (also works with String, Char, etc. and any Tuple/List/union type built up of them) :

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

### _ 

In many functional languages, _ is a wildcard match: it will match anything but importantly (and contrary to Python or Javascript for instance) it will not create a binding. Which means you can match to multiple _ in a single pattern.

```elm
let
  (a,b,_) = myTuple
in
  a ++ b
-- "AB" : String
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

### on a Tuple

```elm
sum (a, b) = a + b
toPrint = (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
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

---

You can pattern match in function definitions when there's only one case. This function takes one tuple rather than two arguments.
```elm
area (width, height) = width * height
area (6, 7) -- 42
```



### on a List

```elm
case aList of
  [] -> "matches the empty list"
  [x]-> "matches a list of exactly one item, " ++ toString x
  x::xs -> "matches a list of at least one item whose head is " ++ toString x
```

Pattern matches go in order. If we put [x] last, it would never match because
x::xs also matches (xs would be the empty list). Matches do not "fall through".
The compiler will alert you to missing or extra cases.

----

Lists themselves can also be used in pattern matching. You can match with the empty list [] or any pattern that involves :: and the empty list. But since [1,2,3] is just syntactic sugar for 1::2::3::[], you can also use the former pattern. A pattern like x::xs will bind the head of the list to x and the rest of it to xs, even if there's only one element so xs ends up being an empty list.

*Note*: The x::xs pattern is used a lot, especially with recursive functions. But patterns that have :: in them only match against lists of length 1 or more.

If you want to bind, say, the first three elements to variables and the rest of the list to another variable, you can use something like x::y::z::zs. It will only match against lists that have three elements or more.

Now that we know how to pattern match against list, let's make our own implementation of the List.member function, which checks if a value is present in a list, using

```elm
member' : a -> List a -> Bool
member' value list =
    case list of
        [] -> False
        (x::xs) ->
            if x == value then
                True
            else
                member' value xs
```

Checking if it works:

```elm
toPrint = member' 1 [1,2,3,4]
True
toPrint = member' 1 [2,3,4,5]
False
```

Nice! Notice that if you want to bind to several variables (even if one of them is just \_ and doesn't actually bind at all), we have to surround them in parentheses. Also notice the if then else expression we used. It's very similar to if statements in other languages, except it's an expression instead of a statement. That means it evaluates to a specific value, so we can't ignore the else.

Let's make a trivial function that tells us some of the first elements of the list in (in)convenient English form.

```elm
tell : List a -> String
tell list =
    case list of
        [] -> "The list is empty"
        (x::[]) -> "The list has one element: " ++ toString x
        (x::y::[]) -> "The list has two elements: " ++ toString x ++ " and " ++ toString y
        (x::y::_) -> "This list is long. The first two elements are: " ++ toString x ++ " and " ++ toString y
```

This function is safe because it takes care of the empty list, a singleton list, a list with two elements and a list with more than two elements. Note that (x::[]) and (x::y::[]) could be rewriten as [x] and [x,y] (because its syntatic sugar, we don't need the parentheses). We can't rewrite (x::y::\_) with square brackets because it matches any list of length 2 or more.

This type of function construction is very common in Elm. Let's get some more practice by using pattern matching and a little recursion to implement our own List.length function:

```elm
length' : List a -> Int
length' list =
    case list of
        [] -> 0
        (_::xs) -> 1 + length' xs
```

This is similar to the factorial and member functions we wrote earlier. First we defined the result of a known input — the empty list. This is also known as the edge condition. Then in the second pattern we take the list apart by splitting it into a head and a tail. We say that the length is equal to 1 plus the length of the tail. We use \_ to match the head because we don't actually care what it is. Also note that we've taken care of all possible patterns of a list. The first pattern matches an empty list and the second one matches anything that isn't an empty list.

Let's see what happens if we call length' on ['h','a','m']. First, it will check if it's an empty list. Because it isn't, it falls through to the second pattern. It matches on the second pattern and there it says that the length is 1 + length' ['a','m'], because we broke it into a head and a tail and discarded the head. O-kay. The length' of ['a','m'] is, similarly, 1 + length' ['m']. So right now we have 1 + (1 + length' ['m']). length' ['m'] is 1 + length' []. And we've defined length' [] to be 0. So in the end we have 1 + (1 + (1 + 0)).

Let's implement List.sum. We know that the sum of an empty list is 0. We write that down as a pattern. And we also know that the sum of a list is the head plus the sum of the rest of the list. So if we write that down, we get:

```elm
sum' : List number -> number
sum' list =
    case list of
        [] -> 0
        (x::xs) -> x + sum' xs
```

One more thing — you can't use ++ in pattern matches. If you tried to pattern match against (xs ++ ys), what would be in the first and what would be in the second list? It doesn't make much sense. It would make sense to match stuff against (xs ++ [x,y,z]) or just (xs ++ [x]), but because of the nature of lists, you can't do that.

*Note:* Not only can we call functions as infix with backticks, we can also define them using backticks. Sometimes it's easier to read that way.

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))


### on a record

```elm
-- Requires the argument with x and y fields
multiply {x,y} = x * y
```

Use curly brackets to pattern match record field names.
Use let to define intermediate values.

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




