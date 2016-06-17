**Elm 0.??**, **copied, please ignore** 

## Tuples

Tuples are denoted with () and their values are separated by commas. The length is fixed. Tuples they don't have to be *homogenous*. Unlike a list, a tuple can contain a combination of several types.

```elm
("elm", 42)
```

The type of a tuple records the number of components s well as the types of the components..

```elm
> (1, "2", True)
(1,"2",True) : ( number, String, Bool )
```

(,), (,,), (,,,), etc.

### Creating tuples

There’s more than one way to create a tuple.

```elm
-- We can write it out explicitly.
yeOldeBasicCoord = (1, 0, 2)
-- We can also use a prefix operator, `,`, to construct the tuple.
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

### Tuples



Think about how we'd represent a two-dimensional vector in Elm. One way would be to use a list. That would kind of work. So what if we wanted to put a couple of vectors in a list to represent points of a shape on a two-dimensional plane? We could do something like `[[1,2],[8,11],[4,5]]`. The problem with that method is that we could also do stuff like `[[1,2],[8,11,5],[4,5]]`, which Elm has no problem with since it's still a list of lists with numbers but it kind of doesn't make sense. But a tuple of size two (also called a pair) is its own type, which means that a list can't have a couple of pairs in it and then a triple (a tuple of size three), so let's use that instead. Instead of surrounding the vectors with square brackets, we use parentheses: `[(1,2),(8,11),(4,5)]`. What if we tried to make a shape like `[(1,2),(8,11,5),(4,5)]`? Well, we'd get this error:

    The 2nd element of this list is an unexpected type of value.
    3| show [(1,2),(8,11,5),(4,5)]

    All elements should be the same type of value so that we can iterate over the
    list without running into unexpected values.

    As I infer the type of values flowing through your program, I see a conflict
    between these two types:

        \_Tuple2
        (number)


It's telling us that we tried to use a pair and a triple in the same list, which is not supposed to happen. You also couldn't make a list like `[(1,2),("One",2)]` because the first element of the list is a pair of numbers and the second element is a pair consisting of a string and a number. Tuples can also be used to represent a wide variety of data. For instance, if we wanted to represent someone's name and age in Elm, we could use a triple: `("Christopher", "Walken", 55)`. As seen in this example, tuples can also contain lists or strings.

Use tuples when you know in advance how many components some piece of data should have. Tuples are much more rigid because each different size of tuple is its own type, so you can't write a general function to append an element to a tuple — you'd have to write a function for appending to a pair, one function for appending to a triple, one function for appending to a 4-tuple, etc.

While there are singleton lists, there's no such thing as a singleton tuple. It doesn't really make much sense when you think about it. A singleton tuple would just be the value it contains and as such would have no benefit to us.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

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

----

You can also pair up types into tuples, for example (`Int`, `Bool`) . This expands to arbitrarily many elements, i.e. (`Int`, `Float`, `Int`) is a 3-tuple with first element `Int` , second `Float` , third `Int` .
