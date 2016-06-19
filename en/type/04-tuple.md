**Elm 0.17**, **copied, please ignore** 

## Tuples

Tuples are denoted with () and their values are separated by commas. They are used when you know in advance how many components some piece of data should have. The length is fixed. Unlike a list, , tuples don't have to be *homogenous*; their elements can be of different types.

```elm
$ elm repl
> ("elm", 42)
> ("Christopher", "Walken", 55)
```

### Type annotation

The type of a tuple records the number of components as well as the types of the components.

```elm
$ elm repl
> (1, "2", True)
(1,"2",True) : ( number, String, Bool )
```

The number of elements must be constant.  

```elm
$ elm repl
> [(1,2),(8,11),(4,5)]
[(1,2),(8,11),(4,5)] : List ( number, number' )
> [(1,2),(8,11,5),(4,5)]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
```
As must the type in any given position.

```elm
$ elm repl
> [(1,2),(8,"11"),(4,5)]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
> [(1,"2"),(8,"11"),(4,"5")]
[(1,"2"),(8,"11"),(4,"5")] : List ( number, String )
```

### Special cases

#### tuple of size one

```elm
$ elm repl
> (5)
5 : number
```

It doesn't make sense to have a tuple of size one as it stores the value it contains.


#### tuple with no value


```elm
$ elm repl
> ()
() : ()
> (,)
<function> : a -> b -> ( a, b )
> (,,)
<function> : a -> b -> c -> ( a, b, c )
```

`()` is called a unit type. See chapter on [Unit Type](10-unit type.md)

`(,)` returns a function that can be used to create a tuple. See creating tuple below.


### Creating tuples

There’s more than one way to create a tuple.

```elm
-- We can write it out explicitly.
yeOldeBasicCoord = (1, 0, 2)
-- We can also use a prefix operator, `(,)`, to construct the tuple.
yeFahncyCoord = (,,) 1 0 2
```

Also possible, put as many commas as you'll have values inside a tuple

```elm
> (,,,) 1 True 'a' []
(1,True,'a',[]) : ( number, Bool, Char, List a )
```
(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))


### Operations on tuples

You can also pair up types into tuples, for example `(Int, Bool)`. This expands to arbitrarily many elements, i.e. `(Int, Float, Int)` is a 3-tuple with first element `Int`, second `Float`, third `Int`.

```elm
myTuple : (String, Int, Bool)
myTuple = ("the answer", 42, True)
```
(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))

Because each different size of tuple is its own type, you cannot really write general functions for tuples. You have to write functions for that target a given length. For, instance, to append an element to a tuple, you'd have to write a function for appending to a pair, one function for appending to a triple, one function for appending to a 4-tuple, etc.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


#### fst and snd

In tuples, the meaning of the fields comes from their order. 

fst : (a, b) -> a. Returns the first element.
snd : (a, b) -> b. Returns the second element.

To access the values of a Tuple, we are given a couple of helpers. fst (1,0) gives us the first value, 1, and snd (1,0) gives us the second value, 0.

Access the elements of a pair with the first and second functions. (This is a shortcut; we'll come to the "real way" in a bit.)

```elm
fst ("elm", 42) -- "elm"
snd ("elm", 42) -- 42
```

---


Two useful functions that operate on pairs:

`fst` takes a pair and returns its first component.

```elm
show fst (8,11) -- 8
show fst ("Wow", False) -- "Wow"
```

`snd` takes a pair and returns its second component. Surprise!

```elm
show snd (8,11) -- 11
show snd ("Wow", False) -- False
```

*Note:* these functions operate only on pairs. They won't work on triples, 4-tuples, 5-tuples, etc. We'll go over extracting data from tuples in different ways a bit later.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### comparison

Tuples are comparable.

#### destructuring / pattern matching

Pattern matching in `case` expressions can be used on tuples. 

----------

```elm
(x, y) = (1, 2)
> x
1 : number
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

----------

```elm
myTuple = ("A", "B", "C")
myNestedTuple = ("A", "B", "C", ("X", "Y", "Z"))

let
  (a,b,c) = myTuple
in
  a ++ b ++ c
-- "ABC" : String

let
  (a,b,c,(x,y,z)) = myNestedTuple
in
  a ++ b ++ c ++ x ++ y ++ z
-- "ABCXYZ" : String
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

Make sure to match every tuple(no more no less) or you will get an error like:

```elm
let
  (a,b) = myTuple
in
  a ++ b
-- TYPE MISMATCH :(
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))


It's also more elegant to declare some constant of your app using destructuring.

```elm
-- with no destructuring
width = 200
height = 100

-- with destructuring
(width, height) = (200, 100)
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

Thanks to @robertjlooby, I learned that we can match exact value of comparable. This is useful when you want to explicitly renaming the variable in your branches of case .. of.

```elm
isOrdered : (String, String, String) -> String
isOrdered tuple =
 case tuple of
  ("A","B","C") as orderedTuple ->
    toString orderedTuple ++ " is an ordered tuple."

  (_,_,_) as unorderedTuple ->
    toString unorderedTuple ++ " is an unordered tuple."


isOrdered myTuple
-- "(\"A\",\"B\",\"C\") is an ordered tuple."

isOrdered ("B", "C", "A")
-- "(\"B\",\"C\",\"A\") is an unordered tuple."
Exact values of comparables can be used to match when destructuring (also works with String, Char, etc. and any Tuple/List/union type built up of them) - @robertjlooby
```

(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

-----

We can write our own function for accessing the third value in a tuple, if we want:

```elm
{- "_" here is used to indicate that we don't use the first two values from the tuple. -}
z : (Float, Float, Float) -> Float
z (_, _, thirdVal) =
    thirdVal
```

We can write out a function that uses more than one value from a tuple:

```elm
{- Note the type signature here! Tuples' values don't all need to be of the same type. -}
formatNameAndAge : (String, String, Int) -> String
formatNameAndAge (firstName, lastName, age) =
    firstName ++ " " ++ lastName ++ " (" ++ (toString age) ++ ")"
```

So suppose we wanted to refer to author Neil Gaiman in this text: we might say `formatNameAndAge ("Neil", "Gaiman", 55), or "Neil Gaiman (55)"`.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

