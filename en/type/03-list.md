**Elm 0.??**, **copied, please ignore** 

## List

Lists are just linked lists (ordered collections where each node of the collection points to the subsequent node, if there is one), where all the values must be same type. You get head and tail, just like you’d expect.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

A `List` holds a collection of related values separated by commas and enclosed in
square brackets. All the values in a list must have the same type:

```elm
$ elm repl
> ["the", "quick", "brown", "fox"]
["the","quick","brown","fox"] : List String
> [1, 2, 3, 4, 5]
[1,2,3,4,5] : List number
> [1, "2", 3, 4, 5]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
> []
[] : List a
```

Functions that operate on lists live in the List module. Use `List.functionName` to access them. 

### Creating Lists

A list can be built in many different ways. 

```elm
$ elm repl
> [1,2,3,4]
> [1..4] -- Texas range
> 1 :: [2,3,4]
> 1 :: 2 :: 3 :: 4 :: []
[1,2,3,4] : List number
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


### Manipulating Lists

#### append one element

To add one item, use "cons".

```bash
$ elm repl
> 0 :: [1..5]
[0,1,2,3,4,5] : List number
```

#### merge two lists

```bash
$ elm repl
> [1..5] ++ [6..10]
[1,2,3,4,5,6,7,8,9,10] : List number
```

#### Destructuring

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

#### iterate through list
```elm
length : List a -> Int
length list =
  case list of
    [] ->
        0
    first :: rest ->
        1 + length rest
```

(source: ???)

#### head and tail

`head` gives you a Maybe with the value of the first element. `tail` gives you the rest of the List. (source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

The head and tail of a list are returned as a Maybe. Instead of checking
every value to see if it's null, you deal with missing values explicitly.

```elm
List.head [1..5] -- Just 1
List.tail [1..5] -- Just [2, 3, 4, 5]
List.head [] -- Nothing
```

### Square Brackets and What’s Inside Them



Want to add an element? Cons `::` is an infix operator that adds to the front of the List. You can concat, append, or intersperse. Or partition or unzip or pretty much whatever else you can think of to do. My point: yay lists!

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

### List Operations in Practice with Elm Html

Lists are vital for something I do a lot of: writing HTML in Elm through the elm-html package. The elm-html DSL is very clean: the functions generating HTML nodes are named familiarly (e.g., div, span) and take two lists as arguments, which leads to very readable code.

When using the node-generating Html functions, the first argument is a list of attributes on that node (we also have functions like id and class for producing these attributes). The second argument is a list of Html–that is, a list of children.

So, something like this is reasonable:

```elm
renderHello : Html
renderHello =
    div
        [ class "hello" ]    -- We could also throw in a click listener here, or add our own custom attribute or property
        [ text "Hello world!"
        , span [] [ text "oh look I'm a child of the div" ]
        ]
```

Reasonable, but boring. Let’s go back to our library.

```elm
type alias LibraryEntry =
    { name : String
    , author : String
    , readByMe : Bool
    }

wutheringHeights =
    LibraryEntry "Wuthering Heights" "Bronte" True


diaspora =
    LibraryEntry "Diaspora" "Egan" False
```

We need more books. And do we really need to have wutheringHeights and diaspora floating around cluttering the namespace? Let’s make a list of our books instead of naming each individually.

```elm
myListOfBooks : List LibraryEntry
myListOfBooks =
    [ LibraryEntry "Wuthering Heights" "Bronte" True
    , LibraryEntry "Diaspora" "Egan" True
    , LibraryEntry "A Handmaid's Tale" "Atwood" True
    , LibraryEntry "Doors of Stone" "Rothfuss" False
    , LibraryEntry "The Taming of the Shrew" "Shakespeare" False
    , LibraryEntry "The Outsiders" "Hinton" True
    , LibraryEntry "A Scanner Darkly" "Dick" True
    ]
Let’s create a function for displaying a single book:
viewBook : LibraryEntry -> Html
viewBook {name, author, readByMe} =
    div
        [ classList [ ("is-unread", readByMe) ] ] {- classList takes List (String, Bool),
                                                     where each tuple represents a className and whether to apply it -}
        [ div [ class "book-name" ] [ text name ]
        , div [ class "book-author" ] [ text author ]
        ]
```

Great! Now how should we display our bookList?

Our bookList is just a list of LibraryEntry’s, and the second argument to the containing element needs to be a List of Html… We just need to map from data to Html directly.

My favorite style is to use the |> operator. This style makes it very easy to follow what data is important and most complex. This version of viewBookList emphasizes that the contents of the “book-collection-container” div is just a transformation of bookList.

```elm
viewBookList : List (LibraryEntry) -> Html
viewBookList bookList =
    bookList
        |> List.map viewBook
        |> div [ class "book-collection-container" ]
```

Here’s another version of the same function. This version mirrors the familiar shape of HTML more closely. Some disadvantages: if we decide later to add another mapping or operation on the bookList, it will be harder to add if using the style below (above, it’s just adding a line: List.filter .readByMe, for instance).

```elm
viewBookList : List (LibraryEntry) -> Html
viewBookList bookList =
    div
        [ class "book-collection-container" ]
        (List.map viewBook bookList)
```        

Having our data stored in a List means that it is very, very easy to map from data -> view. And immutability means that we can do whatever we want to our Lists without worrying about causing unintended changes somewhere.

The ease with which Lists can be manipulated can be helpful with odd-shaped data. For instance, suppose I want to combine my bookList with someone else’s bookList.

```elm
viewTwoBookList : List (LibraryEntry) -> List (LibraryEntry) -> Html
viewTwoBookList bookList willsBookList =
    bookList
        |> List.append willsBookList
        |> List.map viewBook
        |> div [ class "book-collection-container" ]
```

I could also create my Lists of Html with viewBook, and then append the Lists.

Suppose I stole one of Will’s books. I don’t want to store it in myListOfBooks, because it’s not mine, but I do want this one book to show up in my Html. I can add this book I’ve stolen to the front of my bookList, wherever I’m calling viewBookList:

```elm
viewAllBooks =
    let
        theBookIStole =
            LibraryEntry "Axiomatic" "Egan" True

        completeBookList =
            theBookIStole :: myListOfBooks
            {- This infix operator (::) is pronounced "cons".
               That is what I did to steal his book. ;)
            -}
    in
        viewBookList completeBookList
```

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

One core function worth mentioning is `indexedMap`. Coming from JavaScript, this may feel like it’s going to be extremely important. In JS, I found that I often used indices as a key way to access my data (this is a joke. Key? Get it? Because Arrays are just fancy Objects?). Depending on what you’re doing, this function might be helpful to you too.

Maybe you want to zebra-stripe your book entries in a silly way (no nth-child(2n+1) selectors for you).

```elm
viewBook : Int -> LibraryEntry -> Html
viewBook index {name, author, readByMe} =
    div
        [ classList
            [ ("is-unread", readByMe)
            , ("is-even", ((index + 1) % 2 == 0))
            ]
        ]
        [ div [ class "book-name" ] [ text name ]
        , div [ class "book-author" ] [ text author ]
        ]
```

As you may notice from the modular arithmetic going on in this example, Lists in Elm are 0-indexed.

Other functions to be aware of are concatMap and filterMap. Both are semantically named. concatMap applies a function to each element in a List and concatenates the resultant Lists. filterMap applies a function to each element in a List, and keeps the successes.

As you start doing all kinds of fancy nonsense, take a look at elm-list-extra.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

### An intro to lists

Much like shopping lists in the real world, lists in Elm are very useful. It's a common
used data structure and it can be used in a multitude of different ways
to model and solve a whole bunch of problems. Lists are SO awesome. In
this section we'll look at the basics of lists, as well as strings (which are
similar to lists).

In Elm, lists are a *homogenous* data structure. It stores several
elements of the same type. That means that we can have a list of
integers or a list of characters but we can't have a list that has a few
integers and then a few characters. And now, a list!


```elm
lostNumbers = [4,8,15,16,23,42]

show lostNumbers -- [4,8,15,16,23,42]
```

As you can see, lists are denoted by square brackets and the values in
the lists are separated by commas. If we tried a list like
`[1,2,'a',3,'b','c',4]`, Elm would complain that characters (which
are, by the way, denoted as a character between single quotes) are not
numbers.

A common task is putting two lists together. This is done by using the
`++` operator.

```elm
show [1,2,3,4] ++ [9,10,11,12] -- [1,2,3,4,9,10,11,12]
```

Watch out when repeatedly using the `++` operator on long strings. When
you put together two lists (even if you append a singleton list to a
list, for instance: `[1,2,3] ++ [4]`), internally, Elm has to walk
through the whole list on the left side of `++`. That's not a problem when
dealing with lists that aren't too big. But putting something at the end
of a list that's fifty million entries long is going to take a while.
However, putting something at the beginning of a list using the `::`
operator (also called the cons operator) is instantaneous.

```elm
show 5 :: [1,2,3,4,5] -- [5,1,2,3,4,5]
```

Notice how `::` takes a number and a list of numbers or a character and a
list of characters, whereas `++` takes two lists. Even if you're adding an
element to the end of a list with `++`, you have to surround it with
square brackets so it becomes a list.

`[1,2,3] `is actually just syntactic sugar for `1::2::3::[]`. `[]` is an empty
list. If we prepend 3 to it, it becomes `[3]`. If we prepend 2 to that, it
becomes `[2,3]`, and so on.

*Note:* `[]`, `[[]]` and `[[],[],[]]` are all different things. The first one
is an empty list, the seconds one is a list that contains one empty
list, the third one is a list that contains three empty lists.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


#### TODO section on Nth element?


The lists within a list can be of different lengths but they can't be of
different types. Just like you can't have a list that has some
characters and some numbers, you can't have a list that has some lists
of characters and some lists of numbers.

What else can you do with lists? Here are some basic functions that
operate on lists.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

#### TODO section on Maybe?

`head` takes a list and returns its head. The head of a list is basically
its first element.

```elm
show List.head [5,4,3,2,1] -- Just 5
```

`tail` takes a list and returns its tail. In other words, it chops off a
list's head.

```elm
show List.tail [5,4,3,2,1] -- Just [4,3,2,1]
```


But what happens if we try to get the head of an empty list?

```elm
show List.head [] -- Nothing
```

What is `Nothing`? And why did `head` return `Just 5` before?
We'll talk about that a bit more later, but for now,
think of `Nothing` as a way to indicate when there's no correct value to return,
and `Just` as a way to show that we could return a value in a place
where `Nothing` could have also been returned.


`length` takes a list and returns its length, obviously.

```elm
show List.length [5,4,3,2,1] -- 5
```

`isEmpty` checks if a list is empty. If it is, it returns `True`, otherwise it
returns `False`. Use this function instead of `xs == []` (if you have a list
called `xs`).

```elm
show List.isEmpty [1,2,3] -- False
show List.isEmpty [] -- True
```

`reverse` reverses a list.

```elm
show List.reverse [5,4,3,2,1] -- [1,2,3,4,5]
```

`take` takes a number and a list. It extracts that many elements from the
beginning of the list. Watch.

```elm
show List.take 3 [5,4,3,2,1] -- [5,4,3]
show List.take 1 [3,9,3] -- [3]
show List.take 5 [1,2] -- [1,2]
show List.take 0 [6,6,6] -- []
```

See how if we try to take more elements than there are in the list, it
just returns the list. If we try to take 0 elements, we get an empty
list.

`drop` works in a similar way, only it drops the number of elements from
the beginning of a list.

```elm
show List.drop 3 [8,4,2,1,5,6] -- [1,5,6]
show List.drop 0 [1,2,3,4] -- [1,2,3,4]
show List.drop 100 [1,2,3,4] -- []
```

`maximum` takes a list of stuff that can be put in some kind of order and
returns the biggest element.

`minimum` returns the smallest.

```elm
show List.minimum [8,4,2,1,5,6] -- Just 1
show List.maximum [1,9,2,3,4] -- Just 9
show List.maximum [] -- Nothing
```

What is the largest element of an empty list? There's no value we could
return that would make sense, so we use `Just` and `Nothing` here.

`sum` takes a list of numbers and returns their sum.

`product` takes a list of numbers and returns their product.

```elm
show List.sum [5,2,1,6,3,2,5,7] -- 31
show List.product [6,2,1,2] -- 24
show List.product [1,2,5,6,7,9,2,0] -- 0
```

`member` takes a thing and a list of things and tells us if that thing is a
member of the list.

```elm
show List.member 4 [3,4,5,6] -- True
show List.member 10 [3,4,5,6] -- False
```

Those were a few basic functions that operate on lists. We'll take a
look at more list functions [later](modules#data-list).

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Texas ranges

`[1..5]`

What if we want a list
of all numbers between 1 and 20? Sure, we could just type them all out
but obviously that's not a solution for gentlemen who demand excellence
from their programming languages. Instead, we'll use ranges. Ranges are
a way of making lists that are sequences of numbers.

To make a list containing all the natural numbers from 1 to 20, you just
write `[1..20]`. That is the equivalent of writing
`[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]` and there's no
difference between writing one or the other except that writing out long
enumeration sequences manually is stupid.

```elm
show [1..20] -- [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

### Operations on lists

import List exposing (..)

```
(::) : a -> List a -> List a. "Cons" (add) an element to a list.
head : List a -> a
Return the first element of a list.
tail : List a -> List a
Return the remainder of a list after the head.
member : a -> List a -> Bool Tests whether a value is in a list.
take : Int -> List a -> List a Returns the first n elements of a list.
drop : Int -> List a -> List a
Returns the list with the first n elements removed.
isEmpty : List a -> Bool Tests if a list is empty.
length : List a -> Int Returns the length of a list.
reverse : List a -> List a Reverses a list.
append : List a -> List a -> List a Appends two lists.
concat : List (List a) -> List a Combine a list of lists into a single list.
intersperse : a -> List a -> List a Put an element between all elements of a list.
map : (a -> b) -> List a -> List b
Applies a function to each element of a list, returning a list of results.
filter : (a -> Bool) -> List a -> List a Applies a predicate to each element of a list, retaining those that satisfy the predicate.
foldl : (a -> b -> b) -> b -> L Li is st t a -> b Reduce a list from the left. Specifically, takes a binary functionfab->b, an initial valueb, and a list ofa, producing a single b.
foldl1 : (a -> a -> a) -> L Li is st t a -> a Reduce a non-empty list from the left.
foldr : (a -> b -> b) -> b -> L Li is st t a -> b Reduce a list from the right.
foldr1 : (a -> b -> b) -> b -> L Li is st t a -> b Reduce a non-empty list from the right.
scanl : (a -> b -> b) -> b -> L Li is st t a -> List b
Reduce a list from the left, building up all of the intermediate results into a list.
scanl1 : (a -> b -> b) -> b -> List a -> List b
Reduce a non-empty list from the left, building up all of the intermediate results into a list.
map2 : (a -> b -> result) -> List a -> List b -> List result
Combine two lists, element-wise, with the given function. The functions map3, map4, and map5also exist. zip
Not currently in Elm; use map2 (,).
unzip : List (a, b) -> (List a, List b)
Given a list of tuples, returns a tuple of lists.
partition : (a -> Bool) -> List a -> (List a, List a)
Returns a tuple of the elements that satisfy the predicate and those that fail the predicate.
all : (a -> Bool) -> List a -> Bool
Tests whether all elements satisfy the predicate.
any : (a -> Bool) -> List a -> Bool
Tests whether any element satisfies the predicate.
sum : List number -> number Returns the sum of the list elements.
product : List number -> number Returns the product of the list elements.
maximum : List comparable -> Maybe comparable
Returns the largest value in the list.
minimum : List comparable -> Maybe comparable
Returns the smallest value in the list.
sort : List comparable -> List comparable
Sorts from lowest to highest.
sortBy : (a -> comparable) -> List a -> List a
Sorts by a property of the list elements, such as a field of a record.
sortWith : (a -> a -> Order) -> List a -> List a
Sorts according to the supplies function.
filterMap : (a -> Maybe b) -> List a -> List b
Apply a function to a list and keep only the ones that succeed in returning a value.
concatMap : (a -> List b) -> List a -> List b
Map a function onto a list and flatten the results ("flatmap" in some languages).
indexedMap : (Int -> a -> b) -> List a -> List b
Same as map but the function is also applied to the index of each element (starting at zero).
```