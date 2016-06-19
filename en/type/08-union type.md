**Elm 0.17**, **copied, please ignore** 

## Union Types


A union type defines a custom type that can be one of many possibilities. Each of the possibilities is represented as a "tag". 

```elm
$ elm repl
> type Direction = North | South | East | West
> East
East : Repl.Direction
```

We define the type `Direction` and we also create and define the values `North`, `East`, `South` and `West`.


Tags are handy to define simple enumerations. They can also carry other values of known type. Each tag then becomes a value or function depending on whether it takes any arguments.

```elm
type CardSuit = Club | Diamond | Spade | Heart
type CardValue = Jack | Queen | King | Ace | Num Int
```

This expresses that a card Suit can either be a Club, Diamond, Spade or Heart. And a card value can be a Jack, a Queen, a King, an Ace or a Num with an integer. So, a Num 2 or Num 5 is a valid Value. (source: [learning-elm-part-1](http://lucasmreis.github.io/blog/learning-elm-part-1/))

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = div [] 
  [ text ( cardSuitToName Club )
  , br [] []
  , text ( cardValueToName Jack )
  , br [] []
  , text ( cardValueToName ( Num 3 ) )
  , br [] []
  , text ( toString firstCard )
  , br [] []
  , text ( toString secondCard )
  ]
-- club
-- Jack
-- 3

type CardSuit = Club | Diamond | Spade | Heart
type CardValue = Jack | Queen | King | Ace | Num Int
cardSuitToName: CardSuit -> String
cardSuitToName suit =
  case suit of
    Club -> "club"
    Diamond -> "diamond"
    Spade -> "spade"
    Heart -> "heart" 
    
cardValueToName: CardValue -> String
cardValueToName value =
  case value of
    Jack -> "Jack"
    Queen -> "Queen"
    King -> "King"
    Ace -> "Ace"
    Num x -> toString x
    
type alias Card = (CardValue, CardSuit)
firstCard = (Jack, Diamond)
secondCard = ((Num 4), Heart)
```



Tags can work recursively.

```elm
type IntTree = Leaf | Node Int IntTree IntTree
root : IntTree
root = Node 7 Leaf Leaf
```

_Union types_ can also use type variables

```elm
type Person a
  = Name String
  | Surname String
  | Age Int
  | About a
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

Or
```elm
type Tree a = Leaf | Node a (Tree a) (Tree a)
Leaf : Tree a
Node : a -> Tree a -> Tree a -> Tree a
```

"The type tree-of-a is a leaf, or a node of a, tree-of-a, and tree-of-a."

The uppercase tags will be matched exactly. The lowercase variables will match anything. Underscore also matches anything, but signifies that you aren't using it.

```elm
leftmostElement : Tree a -> Maybe a
leftmostElement tree =
  case tree of
    Leaf -> Nothing
    Node x Leaf _ -> Just x
    Node _ subtree _ -> leftmostElement subtree
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md) and [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))


### Special Cases

#### warning

```elm
-- oddly enough, the compiler doesn't complain at this
type BadIdeas = Int | Float`
-- but this you should always declare an entirely new tag and specify a value type
type RightWay = ANewTag Int | AnotherNewTag Float
```


#### Type name different from tag name

It is possible to define a union type with a tag the same name as the type. In that case, that name would be both a type and a value or function.

This only makes sense when there is a single tag.

```elm
type Weight = Weight Int

star1 : Weight
star1 = Weight 100
```

### Pattern matching / Destructuring

_Union types_ are tightly coupled with [case-of](#case-of) statement. It's the tags, not the parent type, that are matched against `case` statements. The compiler expects an exhaustive match, that is all tags must be accounted for. 


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

### Further Reading

- [Evan Czaplicki's discussion of union types](https://gist.github.com/evancz/06fe634245a3aab4a61b)
- [elm-explained](https://github.com/niksilver/elm-explained)
- [understanding-the-elm-type-system.html](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html)

