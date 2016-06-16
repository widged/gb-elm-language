**Elm 0.??**, **copied, please ignore** 

# main

Elm automatically calls a main function for us, so we need to define it. And in that main function, we do our program’s output.

# Comments

`--` begins a single line comment.
`{- and -}` enclose a multiline comment; these can be nested.

(source: ???)

```elm
-- Single line comment

{-
Multi-line comment  
-}
```

Comments for package [documentation](#documentation)

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Comments

~~~~ {.Elm:hs name="code"}
Single line comments start with two dashes.
{- Multiline comments can be enclosed in a block like this.
{- They can be nested. -}
-}
~~~~

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))







# Scope

## Variable definitions

In Elm as in JavaScript, **scope** refers to what is defined, and where. It is the answer to the question, "where did
this thing come from?", which you will ask constantly when reading examples or someone else's code. Scope also can tell
you why something *isn't* available, what code defines similar values, and where you can find documentation.

Scope in Elm is often simpler than in other languages because ordinary values do not change over time. Not counting
syntax (e.g. `if`, `->`, and brackets), pretty much everything in Elm is either a literal, something you imported, or
something you defined.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Scope.md]])

### Top-Level Definitions

The following code is valid Elm and JS:

```elm
answer = 42
```

Elm is immutable, so it can't vary. Therefore, having it global (at least to the module) is harmless.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Scope.md]])


### Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. In this example, some values are function arguments, some are defined in the `let`, and some (the math operators) are imported automatically from Basics. (And `2` is a literal.)

```elm
distanceFrom (originX, originY) (x, y) =
    let dx = x - originX
        dy = y - originY
    in sqrt (dx^2 + dy^2)
```

After the `let`, you can place as many definitions as you like, just like at the top level. They can be fixed values or functions. You can even write type annotations, although you can't define new types.

The expression after the `in`, where all the definitions are in scope, is what the entire `let` expression becomes. Actually, the definitions are in scope even as you write more definitions. Here's a somewhat contrived example.

```elm
radToDeg rad =
    let piInDegrees = 180
        conversionFactor = piInDegrees/pi
    in conversionFactor * rad
```

Be aware that if you define the same name multiple times, the innermost definition is used. Usually you should just avoid the issue entirely by using unique names.

```elm
foo = 0

silly foo =
  let foo = 12
  in foo

silly 5 == 12 -- True
```

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Scope.md]])


## Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from
anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. In this example, some values are
function arguments, some are defined in the `let`, and some (the math operators) are imported automatically from Basics.
(And `2` is a literal.)

```elm
distanceFrom (originX, originY) (x, y) =
    let dx = x - originX
        dy = y - originY
    in sqrt (dx^2 + dy^2)
```

After the `let`, you can place as many definitions as you like, just like at the top level. They can be fixed values or
functions. You can even write type annotations, although you can't define new types.

The expression after the `in`, where all the definitions are in scope, is what the entire `let` expression becomes.
Actually, the definitions are in scope even as you write more definitions. Here's a somewhat contrived example.

```elm
radToDeg rad =
    let piInDegrees = 180
        conversionFactor = piInDegrees/pi
    in conversionFactor * rad
```

Be aware that if you define the same name multiple times, the innermost definition is used. Usually you should just
avoid the issue entirely by using unique names.

```elm
foo = 0

silly foo =
  let foo = 12
  in foo

silly 5 == 12 -- True
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

### Scope in Elm

In Elm as in JavaScript, **scope** refers to what is defined, and where. It is the answer to the question, "where did
this thing come from?", which you will ask constantly when reading examples or someone else's code. Scope also can tell
you why something *isn't* available, what code defines similar values, and where you can find documentation.

Scope in Elm is often simpler than in other languages because ordinary values do not change over time. Not counting
syntax (e.g. `if`, `->`, and brackets), pretty much everything in Elm is either a literal, something you imported, or
something you defined.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))



# Pipes

Nifty syntax feature every language should have

```haskell
(height 24 (italic (fromString "Hello World!")))
```

Becomes

```haskell
fromString "Hello World!"
|> italic
|> height 24
```

(source: [elm-functionalkats-tutorial](https://github.com/micktwomey/elm-functionalkats-tutorial))

```elm
import Html exposing (text)

type Value = Jack | Queen | King | Ace | Num Int
type Suit = Club | Diamond | Spade | Heart

printSuit suit = toString suit

main =
  Spade
    |> printSuit
    |> text
```

The |> operator chains function calls. In the above line I get Spade, call printSuit with it as a parameter, then get the result of that computation and call the function text with it. It shows "Spade" in the output screen, so it works! :)

(Source: ???)

The result of a function can be piped to the next function, using the |>operator, for example,

```elm
message++" "
|> String.toUpper
|> String.repeat times |> String.trimRight |> Html.text
```

(Source: ???)


## Recursion

We mentioned recursion briefly in the previous chapter. In this chapter,
we'll take a closer look at recursion, why it's important to Elm and
how we can work out very concise and elegant solutions to problems by
thinking recursively.

If you still don't know what recursion is, read this sentence. Haha!
Just kidding! Recursion is actually a way of defining functions in which
the function is applied inside its own definition. Definitions in
mathematics are often given recursively. For instance, the fibonacci
sequence is defined recursively. First, we define the first two
fibonacci numbers non-recursively. We say that *F(0) = 0* and *F(1) =
1*, meaning that the 0th and 1st fibonacci numbers are 0 and 1,
respectively. Then we say that for any other natural number, that
fibonacci number is the sum of the previous two fibonacci numbers. So
*F(n) = F(n-1) + F(n-2)*. That way, *F(3)* is *F(2) + F(1)*, which is
*(F(1) + F(0)) + F(1)*. Because we've now come down to only
non-recursively defined fibonacci numbers, we can safely say that *F(3)*
is 2. Having an element or two in a recursion definition defined
non-recursively (like *F(0)* and *F(1)* here) is also called the *edge
condition* and is important if you want your recursive function to
terminate. If we hadn't defined *F(0)* and *F(1)* non recursively, you'd
never get a solution any number because you'd reach 0 and then you'd go
into negative numbers. All of a sudden, you'd be saying that *F(-2000)*
is *F(-2001) + F(-2002)* and there still wouldn't be an end in sight!

Recursion is important to Elm because unlike imperative languages,
you do computations in Elm by declaring what something *is* instead
of declaring *how* you get it. That's why there are no while loops or
for loops in Elm and instead we many times have to use recursion to
declare what something is.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))


## Maximum awesome

The maximum function takes a list of things that can be compared
(e.g. comparables) and returns the biggest of them. Think
about how you'd implement that in an imperative fashion. You'd probably
set up a variable to hold the maximum value so far and then you'd loop
through the elements of a list and if an element is bigger than then the
current maximum value, you'd replace it with that element. The maximum
value that remains at the end is the result. Whew! That's quite a lot of
words to describe such a simple algorithm!

Now let's see how we'd define it recursively. We could first set up an
edge condition and say that the maximum of a singleton list is equal to
the only element in it. Then we can say that the maximum of a longer
list is the head if the head is bigger than the maximum of the tail. If
the maximum of the tail is bigger, well, then it's the maximum of the
tail. That's it! Now let's implement that in Elm.

```elm
maximum' : List comparable -> comparable
maximum' list =
    case list of
        [] -> Debug.crash "maximum of empty list"
        [x] -> x
        (x::xs) ->
            let
                maxTail = maximum' xs
            in
                if x > maxTail then
                    x
                else
                    maxTail
```

As you can see, pattern matching goes great with recursion! Most
imperative languages don't have pattern matching so you have to make a
lot of if else statements to test for edge conditions. Here, we simply
put them out as patterns. So the first edge condition says that if the
list is empty, crash! Makes sense because what's the maximum of an empty
list? I don't know. Note that you shouldn't normally use the Debug.crash
function in your code. A better way would be to use the Maybe type, which
we will see more info on later. Elm's built in List.maximum function uses
the Maybe type here instead of crashing. The second pattern also lays out
an edge condition. It says that if it's the singleton list, just give
back the only element.

Now the third pattern is where the action happens. We use pattern
matching to split a list into a head and a tail. This is a very common
idiom when doing recursion with lists, so get used to it. We use a
*let* binding to define maxTail as the maximum of the rest of the
list. Then we check if the head is greater than the maximum of the rest
of the list. If it is, we return the head. Otherwise, we return the
maximum of the rest of the list.

Let's take an example list of numbers and check out how this would work
on them: [2,5,1]. If we call maximum' on that, the first two patterns
won't match. The third one will and the list is split into 2 and [5,1].
The *let* clause wants to know the maximum of [5,1], so we follow that
route. It matches the third pattern again and [5,1] is split into 5 and
[1]. Again, the where clause wants to know the maximum of [1]. Because
that's the edge condition, it returns 1. Finally! So going up one step,
comparing 5 to the maximum of [1] (which is 1), we obviously get back 5.
So now we know that the maximum of [5,1] is 5. We go up one step again
where we had 2 and [5,1]. Comparing 2 with the maximum of [5,1], which
is 5, we choose 5.

An even clearer way to write this function is to use max. max is a
function that takes two comparables and returns the
bigger of them. Here's how we could rewrite maximum' by using max:

```elm
maximum' : List comparable -> comparable
maximum' list =
    case list of
        [] -> Debug.crash "maximum of empty list"
        [x] -> x
        (x::xs) -> max x (maximum' xs)
```

How's that for elegant! In essence, the maximum of a list is the max of
the first element and the maximum of the tail.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))

## A few more recursive functions

Now that we know how to generally think recursively, let's implement a
few functions using recursion. First off, we'll implement List.repeat.
repeat takes an Int and some element and returns a list that has
several repetitions of the same element. For instance, repeat 3 5
returns [5,5,5]. Let's think about the edge condition. My guess is that
the edge condition is 0 or less. If we try to repeat something zero
times, it should return an empty list. Also for negative numbers,
because it doesn't really make sense.

```elm
repeat' : Int -> a -> List a
repeat' n x =
    if n <= 0 then
        []
    else
        x :: repeat' (n-1) x
```

We used an if then else expression here instead of patterns because we're testing for a
boolean condition. If n is less than or equal to 0, return an empty
list. Otherwise return a list that has x as the first element and then x
replicated n-1 times as the tail. Eventually, the (n-1) part will cause
our function to reach the edge condition.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))


Next up, we'll implement List.take. It takes a certain number of elements
from a list. For instance, take 3 [5,4,3,2,1] will return [5,4,3]. If we
try to take 0 or less elements from a list, we get an empty list. Also
if we try to take anything from an empty list, we get an empty list.
Notice that those are two edge conditions right there. So let's write
that out:

```elm
take' : Int -> List a -> List a
take' n list =
    if n <= 0 then
        []
    else
        case list of
            [] -> []
            (x::xs) -> x :: take' (n-1) xs
```

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))

The if expression specifies that if we try to take a 0 or negative
number of elements, we get an empty list. The first case pattern
indicates that if we try to take anything from an empty list, we get an
empty list. The second case pattern breaks the list into a head and a tail.
And then we state that taking n elements from a list equals a list that
has x as the head and then a list that takes n-1 elements from the tail
as a tail. Try using a piece of paper to write down how the evaluation
would look like if we try to take, say, 3 from [4,3,2,1].

List.reverse simply reverses a list. Think about the edge condition. What is
it? Come on ... it's the empty list! An empty list reversed equals the
empty list itself. O-kay. What about the rest of it? Well, you could say
that if we split a list to a head and a tail, the reversed list is equal
to the reversed tail and then the head at the end.

```elm
reverse' : List a -> List a
reverse' list =
    case list of
        [] -> []
        (x::xs) -> reverse' xs ++ [x]
```

There we go!

zip is a common operation in many functional programming languages
that takes two lists and zips them together, but it isn't a built-in
function in Elm. So let's make our own! zip [1,2,3] [2,3] returns
[(1,2),(2,3)], because it truncates the longer list to match the length
of the shorter one. How about if we zip something with an empty list?
Well, we get an empty list back then. So there's our edge condition.
However, zip takes two lists as parameters, so there are actually two
edge conditions.

```elm
zip : List a -> List b -> List (a, b)
zip list1 list2 =
    case (list1, list2) of
        (_, []) -> []
        ([], _) -> []
        ((x::xs), (y:ys)) -> (x, y) :: zip xs ys
```

What's going on here? This is a more complicated case expression than
we've used up to this point. Remember how we said that in between the
case and of keywords is an expression? Well, we can use that to our
advantage here by packing both lists into a tuple and then pattern
matching on that tuple. In fact, we can write any expression here and
the value of that expression is what is pattern matched on below.
We can also deconstruct and bind more than one layer into the matched
data structure, as we see in the third pattern. In this pattern we're
pulling both lists out of the tuple, and at the same time pulling values
out of the two lists and binding them to names. Cool!

The first two patterns say that if the first list or second list is empty,
we get an empty list. The third one says that two lists zipped are equal
to pairing up their heads and then tacking on the zipped tails. Zipping
[1,2,3] and ['a','b'] will eventually try to zip [3] with []. The edge
condition patterns kick in and so the result is (1,'a'):(2,'b'):[],
which is exactly the same as [(1,'a'),(2,'b')].

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))


Quick, sort!
------------

We have a list of items that can be sorted. Their type is one of the
comparable types. And now, we want to sort them! There's a very cool
algoritm for sorting called quicksort. It's a very clever way of sorting
items. While it takes upwards of 10 lines to implement quicksort in
imperative languages, the implementation is much shorter and elegant in
Elm. Quicksort has become a sort of poster child for functional languages.
Therefore, let's implement it here, even though implementing quicksort
in functional languages is considered really cheesy because everyone does
it to showcase how elegant they are.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))

So, the type signature is going to be quicksort : List comparable -\>
List comparable. No surprises there. The edge condition? Empty list, as is expected.

A sorted empty list is an empty list. Now here comes the main algorithm:
*a sorted list is a list that has all the values smaller than (or equal
to) the head of the list in front (and those values are sorted), then
comes the head of the list in the middle and then come all the values
that are bigger than the head (they're also sorted).* Notice that we
said *sorted* two times in this definition, so we'll probably have to
make the recursive call twice! Also notice that we defined it using the
verb *is* to define the algorithm instead of saying *do this, do that,
then do that ...*. That's the beauty of functional programming! How are
we going to filter the list so that we get only the elements smaller
than the head of our list and only elements that are bigger?
Well, there's a function called List.filter. Its type is
List.filter : (a -> Bool) -> List a -> List a.
So, let's dive in and define this function.

```elm
quicksort : List comparable -> List comparable
quicksort list =
    case list of
        [] -> []
        (x::xs) ->
            let
                smallerSorted = quicksort (List.filter ((>) x) xs)
                biggerSorted = quicksort (List.filter ((<=) x) xs)
            in  
                smallerSorted ++ [x] ++ biggerSorted
```

Let's give it a small test run to see if it appears to behave correctly.

```elm
toPrint = quicksort [10,2,5,3,1,6,7,4,2,3,4,8,9]
[1,2,2,3,3,4,4,5,6,7,8,9,10]
toPrint = String.fromList (quicksort (String.toList "the quick brown fox jumps over the lazy dog"))
"        abcdeeefghhijklmnoooopqrrsttuuvwxyz"
```

Booyah! That's what I'm talking about! So if we have, say
[5,1,9,4,6,7,3] and we want to sort it, this algorithm will first take
the head, which is 5 and then put it in the middle of two lists that are
smaller and bigger than it. So at one point, you'll have [1,4,3] ++ [5]
++ [9,6,7]. We know that once the list is sorted completely, the number
5 will stay in the fourth place since there are 3 numbers lower than it
and 3 numbers higher than it. Now, if we sort [1,4,3] and [9,6,7], we
have a sorted list! We sort the two lists using the same function.
Eventually, we'll break it up so much that we reach empty lists and an
empty list is already sorted in a way, by virtue of being empty. Here's
an illustration:

![quicksort](img/quicksort.png)

An element that is in place and won't move anymore is represented in
orange. If you read them from left to right, you'll see the sorted list.
Although we chose to compare all the elements to the heads, we could
have used any element to compare against. In quicksort, an element that
you compare against is called a pivot. They're in green here. We chose
the head because it's easy to get by pattern matching. The elements that
are smaller than the pivot are light green and elements larger than the
pivot are dark green. The yellowish gradient thing represents an
application of quicksort.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))


Thinking recursively
--------------------

We did quite a bit of recursion so far and as you've probably noticed,
there's a pattern here. Usually you define an edge case and then you
define a function that does something between some element and the
function applied to the rest. It doesn't matter if it's a list, a tree
or any other data structure. A sum is the first element of a list plus
the sum of the rest of the list. A product of a list is the first
element of the list times the product of the rest of the list. The
length of a list is one plus the length of the tail of the list.
Et cetera, et cetera ...

Of course, these also have edge cases. Usually the edge case is some
scenario where a recursive application doesn't make sense. When dealing
with lists, the edge case is most often the empty list. If you're
dealing with trees, the edge case is usually a node that doesn't have
any children.

It's similar when you're dealing with numbers recursively. Usually it
has to do with some number and the function applied to that number
modified. We did the factorial function earlier and it's the product of
a number and the factorial of that number minus one. Such a recursive
application doesn't make sense with zero, because factorials are defined
only for positive integers. Often the edge case value turns out to be an
identity. The identity for multiplication is 1 because if you multiply
something by 1, you get that something back. Also when doing sums of
lists, we define the sum of an empty list as 0 and 0 is the identity for
addition. In quicksort, the edge case is the empty list and the identity
is also the empty list, because if you add an empty list to a list, you
just get the original list back.

So when trying to think of a recursive way to solve a problem, try to
think of when a recursive solution doesn't apply and see if you can use
that as an edge case, think about identities and think about whether
you'll break apart the parameters of the function (for instance, lists
are usually broken into a head and a tail via pattern matching) and on
which part you'll use the recursive call.

(source: [learnyouanelm-05](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/05-recursion.md))

# Pattern matching

This chapter will cover some of Elm's cool syntactic constructs and we'll start with pattern matching. Pattern matching consists of specifying patterns to which some data should conform and then checking to see if it does and deconstructing the data according to those patterns.

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

Many imperative languages (C, C++, Java, etc.) have case syntax and if you've ever programmed in them, you probably know what it's about. It's about taking a variable and then executing blocks of code for specific values of that variable and then maybe including a catch-all block of code in case the variable has some value for which we didn't set up a case.

Elm takes that concept and one-ups it. Like the name implies, case expressions are, well, expressions. Not only can we evaluate expressions based on the possible cases of the value of a variable, we can also do pattern matching. This leads to really neat code that's simple and readable. You can pattern match on any data type — numbers, characters, lists, tuples, etc. Let's make a really trivial function that checks if the number we supplied to it is a seven or not.

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

This is the first time we've defined a function recursively. Recursion is important in functional languages and we'll take a closer look at it later. But in a nutshell, this is what happens if we try to get the factorial of, say, 3. It tries to compute 3 \* factorial 2. The factorial of 2 is 2 \* factorial 1, so for now we have 3 \* (2 \* factorial 1). factorial 1 is 1 \* factorial 0, so we have 3 \* (2 \* (1 \* factorial 0)). Now here comes the trick — we've defined the factorial of 0 to be just 1 and because it encounters that pattern before the catch-all one, it just returns 1. So the final result is equivalent to 3 \* (2 \* (1 \* 1)). Had we written the second pattern on top of the first one, it would catch all numbers, including 0 and our calculation would never terminate. That's why order is important when specifying patterns and it's always best to specify the most specific ones first and then the more general ones later.

Pattern matching must be complete. If we define a function like this:

```elm
charName : Char -> String
charName c =
    case c of
        'a' -> "Albert"
        'b' -> "Broseph"
        'c' -> "Cecil"
```

the compiler will notice that we haven't accounted for all possibilites,
either by specifying a pattern for every possible value of c, or by
including a wildcard that matches on every value. It will say:

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

It complains that we have missing patterns, and rightfully so.
When making patterns, we should always include a catch-all pattern so
that our program doesn't crash if we get some unexpected input. Features
like this are why Elm can all but guarantee no runtime errors in
your code.

As you can see, the syntax for case expressions is pretty simple:

```elm
case expression of
    pattern -> result
    pattern -> result
    pattern -> result
    ...
```

expression is matched against the patterns. The pattern matching action is the same as expected: the first pattern that matches the expression is used. If it falls through the whole case expression and no suitable pattern is found, a compilation error occurs.

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

## Let it be

Let bindings let you bind to variables anywhere and are expressions themselves, so they are very useful for naming the results of more complicated expressions. Just like any construct in Elm that is used to bind values to names, let bindings can be used for pattern matching. Let's see them in action! This is how we could define a function that gives us a cylinder's surface area based on its height and radius:

```elm
cylinder : Float -> Float -> Float
cylinder r h =
    let
        sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  
        sideArea + 2 * topArea
```

The form is let \<bindings\> in \<expression\>. The names that you define in the *let* part are accessible to the expression after the *in* part. Notice that the names are also aligned in a single column.

Let bindings are expressions themselves. Remember when we did the if statement and it was explained that an if then else statement is an expression? That means you can cram it in almost anywhere.

```elm
toPrint = [if 5 > 3 then "Woo" else "Boo", if 'a' > 'b' then "Foo" else "Bar"]
["Woo", "Bar"]
toPrint = 4 * (if 10 > 5 then 10 else 0) + 2
42
```

You can also do that with let bindings.

```elm
toPrint = 4 * (let a = 9 in a + 1) + 2
42
```

They can also be used to introduce functions in a local scope:

```elm
toPrint = [let square x = x * x in (square 5, square 3, square 2)]
[(25,9,4)]
```

Like we said before, you can pattern match with *let* bindings. They're very useful for quickly dismantling a tuple into components and binding them to names and such.

```elm
toPrint = (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))


# Eager by default


Elm syntax is very similar to Haskell. There is one important difference to keep in mind. Elm is *eager*, unlike Haskell, which is lazy. That means, in Elm, if you call a function, the arguments are fully evaluated before they are passed to the function. Most programming languages are like this, so you won't likely need to think about this very much.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/01-introduction.md))

# Pattern matching

Pattern matching on record fields
```elm
-- Requires the argument with x and y fields
multiply {x,y} =
    x * y
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

# Links

* [elm-lang style guide](http://elm-lang.org/docs/style-guide)
* [elm style guide @NodeRedInk](https://github.com/NoRedInk/elm-style-guide)
*
