**Elm 0.17**, **almost shareable** 

## List

Lists are denoted by square brackets and the values in the lists are separated by commas.  In Elm, Lists are *homogenous* data structures. All the values in a list must have the same type. 


```elm
$ elm repl
> ["the", "quick", "brown", "fox"]
["the","quick","brown","fox"] : List String
> [1, 2, 3, 4, 5]
[1,2,3,4,5] : List number
> [1, "2", 3, 4, 5]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
> [] -- empty list
[] : List a
```

Functions that operate on lists live in the List module. Use `List.functionName` to access them. 

Note. strings are not Lists of characters, though they are similar with Lists.

Note2. The constrain of *homogeneity* applies to list within a list as well.They can be of different lengths but they can't be of different types. You can't have a list that has some lists of characters and some lists of numbers. 


### Type annotation



### Creating Lists

A list can be built in many different ways. 

```elm
$ elm repl
> [1,2,3,4]
> [1..4] -- Texas range
> [1,2] ++ [3,4]
> 1 :: [2,3,4]
> 1 :: 2 :: 3 :: 4 :: []
[1,2,3,4] : List number
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### texas ranges

Ranges are a way of making lists that are sequences of numbers. To make a list containing numbers from 1 to 20, you just write `[1..20]`. That is the equivalent of writing `[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]`.

#### merge two lists

Two lists are put together with the `++` operator.

```bash
$ elm repl
> [1,2,3,4,5] ++ [6,7,8,9,10]
[1,2,3,4,5,6,7,8,9,10] : List number
```

A limitation to be aware of is that, internally, Elm walks through the whole list on the left side of `++` before adding any new element. It can become an issue when manipulating very long lists.  


#### putting something at the beginning of a list

To add one item, use the "cons" operator `::`. Cons is an infix operator that adds to the front of the List. While `++` can be problematic with long lists, `::` is instantaneous.

```bash
$ elm repl
> 0 :: [1,2,3,4,5]
[0,1,2,3,4,5] : List number
> 1::2::3::[]
[1,2,3] : List number
```

Notice that `::` takes an element to the left and a list to the right. 

### Manipulating Lists


#### head and tail

Lists are just linked lists (ordered collections where each node of the collection points to the subsequent node, if there is one), where all the values must be same type. You get head and tail, just like you’d expect. (source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

`head` takes a list and returns the first element. `tail` gives you the rest of the List (the list with the head chopped off).

The head and tail of a list are returned as a Maybe. Instead of checking every value to see if it's null, you deal with missing values explicitly. It `head` or `tail` value of an empty list is `Nothing`. It is Just a value for non empty lists.

```elm
$ elm repl
> List.head [5,4,3,2,1]
Just 5 : Maybe.Maybe number
> List.tail [5,4,3,2,1]
Just [4,3,2,1] : Maybe.Maybe (List number)
> List.head []
Nothing : Maybe.Maybe a
> List.tail []
Nothing : Maybe.Maybe (List a)
```

#### destructuring

List have limited support for destructuring. One of the case is used to find the first element of a list by utilizing the cons operator, ie ::w

```elm
myList = ["a", "b", "c"]

first list =
  case list of
    f::_ -> Just f
    [] -> Nothing

first myList
-- Just "a"
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))


This is much more cleaner than using List.head but at the same time increase codebase complexity. By stacking up the :: operator, we can also use it to match second or other value.

```elm
listDescription : List String -> String
listDescription list =
 case list of
    [] -> "Nothing here !"
    [_] -> "This list has one element"
    [a,b] -> "Wow we have 2 elements: " ++ a ++ " and " ++ b
    a::b::_ -> "A huge list !, The first 2 are: " ++ a ++ " and " ++ b
```

(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

You can match lists of definite length with the [] notation and match more than just the head element with multiple :::

```elm
listFn : List String -> String
listFn list =
  case list of
    [] -> "This list is empty!"
    [ _ ] -> "This list has one element"
    [ a, b ] -> "This list has two elements: " ++ a ++ " and " ++ b
    a::b::_ -> "This list has several elements. The first 2 are: " ++ a ++ " and " ++ b
```
(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

#### iterating through values
```elm
length : List a -> Int
length list =
  case list of
    [] ->
        0
    first :: rest ->
        1 + length rest
```


### Operations on lists

You can concat, append, or intersperse. Or partition or unzip or pretty much whatever else you can think of to do. 

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

`import List exposing (..)`


`length` takes a list and returns its length, obviously.

```elm
> List.length [5,4,3,2,1] -- 5
```

`isEmpty` checks if a list is empty. If it is, it returns `True`, otherwise it
returns `False`. Use this function instead of `xs == []` (if you have a list
called `xs`).

```elm
> List.isEmpty [1,2,3] -- False
> List.isEmpty [] -- True
```

`reverse` reverses a list.

```elm
> List.reverse [5,4,3,2,1] -- [1,2,3,4,5]
```

`take` takes a number and a list. It extracts that many elements from the
beginning of the list. Watch.

```elm
> List.take 3 [5,4,3,2,1] -- [5,4,3]
> List.take 1 [3,9,3] -- [3]
> List.take 5 [1,2] -- [1,2]
> List.take 0 [6,6,6] -- []
```

See how if we try to take more elements than there are in the list, it
just returns the list. If we try to take 0 elements, we get an empty
list.

`drop` works in a similar way, only it drops the number of elements from
the beginning of a list.

```elm
> List.drop 3 [8,4,2,1,5,6] -- [1,5,6]
> List.drop 0 [1,2,3,4] -- [1,2,3,4]
> List.drop 100 [1,2,3,4] -- []
```

`maximum` takes a list of stuff that can be put in some kind of order and
returns the biggest element.

`minimum` returns the smallest.

```elm
> List.minimum [8,4,2,1,5,6] -- Just 1
> List.maximum [1,9,2,3,4] -- Just 9
> List.maximum [] -- Nothing
```

What is the largest element of an empty list? There's no value we could
return that would make sense, so we use `Just` and `Nothing` here.

`sum` takes a list of numbers and returns their sum.

`product` takes a list of numbers and returns their product.

```elm
> List.sum [5,2,1,6,3,2,5,7] -- 31
> List.product [6,2,1,2] -- 24
> List.product [1,2,5,6,7,9,2,0] -- 0
```

`member` takes a thing and a list of things and tells us if that thing is a
member of the list.

```elm
> List.member 4 [3,4,5,6] -- True
> List.member 10 [3,4,5,6] -- False
```

Those were a few basic functions that operate on lists. We'll take a
look at more list functions [later](modules#data-list).

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### all 
```
(::) : a -> List a -> List a. "Cons" (add) an element to a list.
head : List a -> a -- Return the first element of a list.
tail : List a -> List a -- Return the remainder of a list after the head.
member : a -> List a -> Bool Tests whether a value is in a list.
take : Int -> List a -> List a Returns the first n elements of a list.
drop : Int -> List a -> List a -- Returns the list with the first n elements removed.
isEmpty : List a -> Bool -- Tests if a list is empty.
length : List a -> Int -- Returns the length of a list.
reverse : List a -> List a -- Reverses a list.
append : List a -> List a -> List a -- Appends two lists.
concat : List (List a) -> List a -- Combine a list of lists into a single list.
intersperse : a -> List a -> List a -- Put an element between all elements of a list.
map : (a -> b) -> List a -> List b -- Applies a function to each element of a list, returning a list of results.
filter : (a -> Bool) -> List a -> List a -- Applies a predicate to each element of a list, retaining those that satisfy the predicate.
foldl : (a -> b -> b) -> b -> L Li is st t a -> b -- Reduce a list from the left. Specifically, takes a binary functionfab->b, an initial valueb, and a list ofa, producing a single b.
foldl1 : (a -> a -> a) -> L Li is st t a -> a -- Reduce a non-empty list from the left.
foldr : (a -> b -> b) -> b -> L Li is st t a -> b -- Reduce a list from the right.
foldr1 : (a -> b -> b) -> b -> L Li is st t a -> b -- Reduce a non-empty list from the right.
scanl : (a -> b -> b) -> b -> L Li is st t a -> List b --  Reduce a list from the left, building up all of the intermediate results into a list.
scanl1 : (a -> b -> b) -> b -> List a -> List b -- Reduce a non-empty list from the left, building up all of the intermediate results into a list.
map2 : (a -> b -> result) -> List a -> List b -> List result -- Combine two lists, element-wise, with the given function. The functions map3, map4, and map5also exist. 
zip -- Not currently in Elm; use map2 (,).
unzip : List (a, b) -> (List a, List b) -- Given a list of tuples, returns a tuple of lists.
partition : (a -> Bool) -> List a -> (List a, List a) -- Returns a tuple of the elements that satisfy the predicate and those that fail the predicate.
all : (a -> Bool) -> List a -> Bool -- Tests whether all elements satisfy the predicate.
any : (a -> Bool) -> List a -> Bool -- Tests whether any element satisfies the predicate.
sum : List number -> number Returns the sum of the list elements.
product : List number -> number Returns the product of the list elements.
maximum : List comparable -> Maybe comparable -- Returns the largest value in the list.
minimum : List comparable -> Maybe comparable -- Returns the smallest value in the list.
sort : List comparable -> List comparable -- Sorts from lowest to highest.
sortBy : (a -> comparable) -> List a -> List a -- Sorts by a property of the list elements, such as a field of a record.
sortWith : (a -> a -> Order) -> List a -> List a -- Sorts according to the supplies function.
filterMap : (a -> Maybe b) -> List a -> List b -- Apply a function to a list and keep only the ones that succeed in returning a value.
concatMap : (a -> List b) -> List a -> List b -- Map a function onto a list and flatten the results ("flatmap" in some languages).
indexedMap : (Int -> a -> b) -> List a -> List b -- Same as map but the function is also applied to the index of each element (starting at zero).
```