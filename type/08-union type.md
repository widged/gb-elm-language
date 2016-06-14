**Elm 0.??**, **copied, please ignore** 

# Union Types

```elm
type Fruit = Apple | Banana | Orange

fruitToColor: Fruit -> String
fruitToColor fruit =
  case fruit of
    Apple -> "green"
    Banana -> "yellow"
    Orange -> "orange"
```

(source: ???)

```elm
type Value = Jack | Queen | King | Ace | Num Int
type Suit = Club | Diamond | Spade | Heart
```

Both types are *union types*. That means that a Suit can either be a Club, Diamond, Spade or Heart. And a Value can be a Jack, a Queen, a King, an Ace or a Num with an integer. So, a Num 2 or Num 5 is a valid Value. That seems like a nice model for our problem. Now let's implement the functions that print a card. ## Printing a Card I'll first print a Suit. It's a no brainer:
printSuit suit = toString suit

(source: [[http://lucasmreis.github.io/blog/learning-elm-part-1/]])

```elm
type alias Point = { x : Float, y : Float }

type Shape
  = Circle Point Float
  | Rectangle Point Point

area : Shape -> Float
area shape =
  case shape of
    Circle center radius ->
      pi * radius ^ 2

    Rectangle corner1 corner2 ->
      abs (corner1.x - corner2.x) * abs (corner1.y - corner2.y)
```

(source: ???)

The second are the tags of a union type. For example, as tree: `type Tree a = Leaf | Node a (Tree a) (Tree a)`. Each tag becomes a value or function (depending on whether it takes any arguments). In this case, we get the value `Leaf : Tree a` and `Node : a -> Tree a -> Tree a -> Tree a`. It's these tags, not the `Tree` type, that are used as pattern matches in `case` statements. Although less common, it's possible to define a union type with a tag the same name as the type. In that case, that name would be both a type and a value or function.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Scope.md]])


## Union types

For example, as tree: `type Tree a = Leaf | Node a (Tree a) (Tree a)`. Each tag becomes a value or function (depending on whether it takes any arguments). In this case, we get the value `Leaf : Tree a` and `Node : a -> Tree a -> Tree a -> Tree a`. It's these tags, not the `Tree` type, that are used as pattern matches in `case` statements. Although less common, it's possible to define a union type with a tag the same name as the type. In that case, that name would be both a type and a value or function.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))


## Union Types


By contrast, defining a union type creates a type that didn't exist before.
A union type is so called because it can be one of many possibilities.
Each of the possibilities is represented as a "tag".

~~~~ {.Elm:hs name="code"}
type Direction =
  North | South | East | West
~~~~

Tags can carry other values of known type. This can work recursively.

~~~~ {.Elm:hs name="code"}
type IntTree =
  Leaf | Node Int IntTree IntTree
~~~~

"Leaf" and "Node" are the tags. Everything following a tag is a type.
Tags can be used as values or functions.

~~~~ {.Elm:hs name="code"}
root : IntTree
root =
  Node 7 Leaf Leaf
~~~~

Union types (and type aliases) can use type variables.

~~~~ {.Elm:hs name="code"}
type Tree a =
  Leaf | Node a (Tree a) (Tree a)
~~~~

"The type tree-of-a is a leaf, or a node of a, tree-of-a, and tree-of-a."
Pattern match union tags. The uppercase tags will be matched exactly. The
lowercase variables will match anything. Underscore also matches anything,
but signifies that you aren't using it.

~~~~ {.Elm:hs name="code"}
leftmostElement : Tree a -> Maybe a
leftmostElement tree =
  case tree of
    Leaf -> Nothing
    Node x Leaf _ -> Just x
    Node _ subtree _ -> leftmostElement subtree
~~~~

(source: ???)

## Union Types
Elm allows to create custom types known as _union types_.<br/>
The expression below creates a type which can have one of the values (or _tags_) from the right. _Union types_ are tightly coupled with [case-of](#case-of) statement.
```elm
type Movement = Right | Left | Stop
```

Tags bring additional information, after tag itself comes a type or multiple types.
```elm
type Movement
    = Right Int
    | Left Int
    | Stop Bool
    | Coordinates (Float, Float)

-- passing to the function
myFunction ( Coordinates (45.7, 67.5) )
```

_Union types_ can also have type variables
```elm
type Person a
  = Name String
  | Surname String
  | Age Int
  | About a
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


## Enumerations

A simple union type allows us to define an enumeration, like this

```elm
type Direction = North | East | South | West
```

In the above example we've defined the type `Direction`, but we've also
created and defined the values `North`, `East`, `South` and `West`.
If we put this code into
[UnionTypes.elm](UnionTypes.elm)
then we can use them like this in the REPL:

```
> import UnionTypes exposing (..)
> East
East : UnionTypes.Direction
>
```

You can see that `East` is a value, and it has type `Direction`.

Here's an example of using this

```elm
describe : Direction -> String
describe dir
    = case dir of
        North -> "Cold"
        East -> "Arid"
        South -> "Hot"
        West -> "Humid"
```

Note that the code above won't compile if we miss out one of the directions,
because the compiler expects an exhaustive match.

(source: [elm-explained](https://github.com/niksilver/elm-explained))


## More elaborate union types

Union types also allow us to use more complex constructors.
The `Character` type defined next is a simple enumeration, but
the `Card` type is a mix of tagged `Value` and tagged `String` types.

```elm
type Character = Ace | King | Queen | Jack
type Value = Pips Int | Name Character
type Card = Heart Value
    | Diamond Value
    | Club Value
    | Spade Value
    | Joker String

d10 = Diamond (Pips 10)
hking = Heart (Name King)
joker1 = Joker "Laughing Jeremy"
```

Let's look at the values' types:

```
> import UnionTypes exposing (..)
> d10
Diamond (Pips 10) : UnionTypes.Card
> hking
Heart (Name King) : UnionTypes.Card
> joker1
Joker ("Laughing Jeremy") : UnionTypes.Card
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

## Union types must create new tags

For a union type to be valid, all the types in the union
must be creating a new type. So we can't do (say)

```elm
type Counter = Int | { name : String, c : Int}
```

But we can do this if `AnonCounter` and `NamedCounter` are entirely
new and haven't been defined already

```elm
type Counter = AnonCounter Int | NamedCounter { name : String, c : Int }
```

See also
[Evan Czaplicki's discussion of union types](https://gist.github.com/evancz/06fe634245a3aab4a61b)

(source: [elm-explained](https://github.com/niksilver/elm-explained))

## It's okay to have a single tag

## Type name different from tag name

We can have the type declaration (the tag)
different from the definition, oddly.
What is the value of this?

```elm
type Laurel = Hardy Int

star1 : Laurel
star1 = Hardy 100
```

## Destructuring

Again, thanks to @robertjlooby, we can even destruct the arguments of union type.

```elm
type MyThing
  = AString String
  | AnInt Int
  | ATuple (String, Int)

unionFn : MyThing -> String
unionFn thing =
  case thing of
    AString s -> "It was a string: " ++ s
    AnInt i -> "It was an int: " ++ toString i
    ATuple (s, i) -> "It was a string and an int: " ++ s ++ " and " ++ toString i
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

You can destructure nested values in tagged union types:

```elm
type MyThing
  = AString String
  | AnInt Int
  | ATuple (String, Int)

unionFn : MyThing -> String
unionFn thing =
  case thing of
    AString s -> "It was a string: " ++ s
    AnInt i -> "It was an int: " ++ toString i
    ATuple (s, i) -> "It was a string and an int: " ++ s ++ " and " ++ toString i
```

(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))


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

Another case union types with only one member:

```elm
type MyThing
  = AString String


unionFn : MyThing -> String
unionFn  (AString a) = a
```

(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))
