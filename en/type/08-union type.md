**Elm 0.17**, **copied, please ignore** 

## Union Types

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


### Union types

For example, as tree: `type Tree a = Leaf | Node a (Tree a) (Tree a)`. Each tag becomes a value or function (depending on whether it takes any arguments). In this case, we get the value `Leaf : Tree a` and `Node : a -> Tree a -> Tree a -> Tree a`. It's these tags, not the `Tree` type, that are used as pattern matches in `case` statements. Although less common, it's possible to define a union type with a tag the same name as the type. In that case, that name would be both a type and a value or function.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

-------

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

------

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


### Enumerations

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


### More elaborate union types

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

### Union types must create new tags

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

### It's okay to have a single tag

### Type name different from tag name

We can have the type declaration (the tag)
different from the definition, oddly.
What is the value of this?

```elm
type Laurel = Hardy Int

star1 : Laurel
star1 = Hardy 100
```

### Destructuring

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

#### Exporting and Importing Union Types

Union types have a little bit of special treatment when it comes to imports and exports. Let's start with a simple union type, which you might find in a counter demo.

`type Action = Increment | Decrement`

Remember that `Increment` and `Decrement` are called "tags". How would we export this type?

`module MyModule exposing (Action)` exports `Action` only. That is called an *opaque type*. Clients can see that `Action` exists, but they can't see into it. This is frequently what you want, and we'll talk about it more in the next section.

`module MyModule exposing (Action(..))` exports `Action` and all of its tags, namely `Increment` and `Decrement`. This form can be useful sometimes if you can commit to keeping `Action` the same. A good example is a [nonempty list](http://package.elm-lang.org/packages/mgold/elm-nonempty-list/latest/List-Nonempty#Nonempty); there will never be a reason to change the type's definition so the tag can be safely exported.

`module MyModule (Action(Increment, Decrement)) where` exports `Action` and only the listed tags. In this case it's listing all the tags explicitly. It's possible to only export *some* of the tags, but there is no reason to do this, ever.

The trouble with exporting tags is not only that you may want to remove some, which will break any code that relies on the ones being removed. Even adding tags will break code, because previously exhaustive pattern matches are no longer exhaustive. If only some of the tags are exported, it's impossible to write a valid `case` statement (at least not without a `_ ->` pattern, which are discouraged).

Importing union types exposed follows the exact same syntax. For example, `import MyModule exposing (Action)` will import only the type, while `import MyModule exposing (Action(..))` will import any exported tags as well. You can also use exported tags qualified, like `MyModule.Increment`.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### Opaque Types

An **opaque type** is a union type where the type is exported but the tag(s) are not. Someone outside the module can see that the type exists, pass it around, and store it in records. They only thing they *can't* do is look inside the type, even if they know what the tags are named. Hence, it's opaque.

An example of an opaque type would be a 2D point. Creating a point would require either `x` and `y`, or `r` and `theta`. Each value could be accessed individually from a given point. The point might actually store all four, knowing that there's no way for someone to create a point that's inconsistent.

What this means is that opaque types are Elm's way of enforcing information hiding. They allow a package author to define an interface of functions to create, update, and read useful values out of the opaque type. The implementation can change completely, but as long as all functions on the type are updated to match, it's still considered a patch change. This gives package writers flexibility when writing their libraries. It also lets them rely on invariants, assured that the client hasn't meddled with values of the type in unexpected ways.

Opaque types are less useful in applications. If you're typing to simply pass information around, exporting record type aliases is fine. If it makes sense to also define operations on these models, an opaque type might be a better fit.

If your union type contains many tags, you can export functions that wrap them. You can be selective about which ones you export. Sometimes it can be tedious, but it's worth it.

```elm
module Road (Action, addCar, redLight) where

import Car exposing (Car)

type Light =
  Red | Yellow | Green

type Action =
  AddCar Car | Light Light | SomethingPrivate

addCar : Car -> Action
addCar =
  AddCar

redLight : Action
redLight =
  Light Red
```

You cannot make an opaque type out of a type alias; those are either exported or not, just like values. But you can create a union type to hide it. (This is more common when the opaque type represents a model rather than an action.)

```elm
module Person (Person, age) where

type Person =
  P { name : String, age : Int }

age : Person -> Int
age (P {age}) =
  age
```

First, we define the union type `Person` with one tag, `P`, which carries a record. (You can put the record definition directly in the union type, or define an unexported alias.) Then we can export the `age` function which accesses the record in a way not possible outside of this module. The definition uses two nifty language features. First, it pattern matches on the `P` tag in the argument list. This is permitted when there is only one tag, because otherwise it's an incomplete pattern match. Next, `{age}` destructures the record, assigning the local constant `age` to the value of the record's `age` field. It's a much more concise way of writing this:

```elm
age person =
  case person of
    P record ->
      record.age
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

With tagged unions, you can define a type that represents the possibility of multiple types. This is one of the key features that supports the Elm architecture. A tagged union will typically be paired with a case statement to build out a function that offers multiple paths through which your program can update. The name sounds strange but the code is simple, and the type system will support you every step of the way. Here is an example:

```elm
type User = SuperAdmin | Admin | Basic

userPhoto : User -> String
userPhoto user =
  case user of
    SuperAdmin ->
      "thassa.png"

    Admin ->
      "jace.png"

    Basic ->
      "merfolk.png"
```


This basic example only scratches the surface of how helpful these types can be. The Elm compiler is there to guide you towards accounting for every scenario so that your customers won’t encounter errors at runtime. Watch what happens when you remove one of the branches from the case statment and try to compile:

```elm
type User = SuperAdmin | Admin | Basic

userPhoto : User -> String
userPhoto user =
  case user of
    Admin ->
      "jace.png"

    Basic ->
      "merfolk.png"
```

The Elm compiler notices when you don't account for all cases. Imagine the power of this when you are working on a code base with multiple modules, or multiple programmers, or when you haven’t looked at the code in a while. Tagged unions are outlined in greater detail in the Elm docs.

(source: [understanding-the-elm-type-system.html](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

### Opaque type

Record-like opaque types don't get the magical accessor functions

```elm
type AThing = AThing { foo: String, bar: Int }

foo (AThing { foo }) = foo
```
(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

