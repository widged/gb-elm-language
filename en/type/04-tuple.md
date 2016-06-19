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

In a program

```elm
userProfile : (String, String, Int)
userProfile = ("John", "Doe", 42)
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

Tuples are typically created by enclosing comma separated values in between parentheses.

```elm
$ elm repl
> (12, 7)
(12,7) : ( number, number' )
```

It is also possible to use a prefix operator, `(,)`, to construct a tuple. The function is a tuple constructor not bound to the value it helps create. 

```elm
$ elm repl
> (,) 12 7
(12,7) : ( number, number' )
> coord = (,)
<function> : a -> b -> ( a, b )
> coord 12 7
(12,7) : ( number, number' )
> coord 12 "7"
(12,"7") : ( number, String )
> (,,,) 1 True 'a' []
(1,True,'a',[]) : ( number, Bool, Char, List a )
```

### Operations on tuples

With tuples, type is bound to the number of elements. Therefore, functions have to target a given length. Different functions must be provided to append an element to a tuple pair, a tuple triple, or a tuple quad.

#### fst and snd of a tuple pair

For tuple pairs, special accessors are given that return the first or second element of the pair. 

```elm
$ elm repl
> pair = (5, 7)
(5,7) : ( number, number' )
> fst pair
5 : number
> triple = (5, 7, 8)
(5,7,8) : ( number, number', number'' )
> fst triple
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The argument to function `fst` is causing a mismatch.
Function `fst` is expecting the argument to be: ( a, b )
```



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

