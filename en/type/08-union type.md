**Elm 0.17**, **shareable** 

## Union Types


A union type defines a custom type that represents the possibility of multiple types. Each of the possibilities is represented as a "tag". 

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

#### only new tags should be used

```elm
-- oddly enough, the compiler doesn't complain at this
type BadIdeas = Int | Float`
-- but this you should always declare an entirely new tag and specify a value type
type RightWay = ANewTag Int | AnotherNewTag Float
```

#### type name is typically different from tag name

It is possible to define a union type with a tag the same name as the type.  However, it generally doesn't make sense. 

The only exception is when there is only a single tag. This is then a special case of [Type Constructor](11-type constructor.md). 

```elm
type Aged = Aged Int
century = Aged 100
```

#### tags that carry a record don't get accessor functions

```elm
> type User = AUser { first: String, last: String, age: Int }
> user = AUser { first = "Jane", last = "Doe", age = 42 }
AUser { first = "Jane", last = "Doe", age = 42 } : Repl.User
> user.foo
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm

`user` is being used in an unexpected way.

6|   user.foo
     ^^^^
Based on its definition, `user` has this type:

    User

But you are trying to use it as:

    { b | foo : a }
```

### Pattern matching / Destructuring

_Union types_ are tightly coupled with a [case-of](#case-of) statement. It's the tags, not the parent type, that are matched against `case` statements. 

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

The compiler expects an exhaustive match, that is all tags must be accounted for. If you leave one out, the Elm compiler will complain. 


### Exporting Union Types and their tags

The trouble with exporting tags is not only that you may want to remove some, which will break any code that relies on the ones being removed. Even adding tags will break code, because previously exhaustive pattern matches are no longer exhaustive. If only some of the tags are exported, it's impossible to write a valid `case` statement (at least not without a `_ ->` pattern, which are discouraged).

In packages, you typically want to avoid exposing the tag values. 

In applications, if you're typing to simply pass information around, exporting record type aliases is fine or the tags of union types is fine.

#### exporting as opaque type with wraping functions

An **opaque type** is a union type where the type is exported but the tag(s) are not. Someone outside the module can see that the type exists, pass it around, and store it in records. They cannot access the tags. Hence, it's opaque.

```elm
module MyModule exposing (Msg)
type Msg = Increment | Decrement
```

Opaque types provide a way of enforcing information hiding. Clients can see that `Msg` exists, but they can't see into it. 

The package author instead defines an interface of functions to create, update, and read useful values out of the opaque type. 

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

```elm
module Road exposing (Msg, addCar, redLight)

import Car exposing (Car)

type Light = Red | Yellow | Green

type Msg = AddCar Car | Light Light | SomethingPrivate

addCar : Car -> Msg
addCar = AddCar

redLight : Msg
redLight = Light Red
```

Sometimes it can be tedious, but it's worth it. The implementation can change completely, but as long as all functions on the type are updated to match, it's still considered a patch change. This gives package writers flexibility when writing their libraries. It also lets them rely on invariants, assured that the client hasn't meddled with values of the type in unexpected ways.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### union type instead of record alias

A _Union Type_ can similarly be used to hide what would have been a type alias for a record shape. (This is more common when the opaque type represents a model rather than an action.)

```elm
module Person exposing (Person, age)
type Person = P { name : String, age : Int }
age : Person -> Int
age (P {age}) = age
```

First, we define the union type `Person` with one tag, `P`, which carries a record. (You can put the record definition directly in the union type, or define an unexported alias.) Then we can export the `age` function which accesses the record in a way not possible outside of this module. The definition uses two nifty language features. First, it pattern matches on the `P` tag in the argument list. This is permitted when there is only one tag, because otherwise it's an incomplete pattern match. Next, `{age}` destructures the record, assigning the local constant `age` to the value of the record's `age` field. It's a much more concise way of writing this:

```elm
age person =
  case person of
    P record ->
      record.age
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### exporting a union type and all of its tags

```elm
module MyModule exposing (Msg (..))
type Msg = Increment | Decrement
```

This form should be used only if you can commit to keeping `Msg` the same. A good example is a [nonempty list](http://package.elm-lang.org/packages/mgold/elm-nonempty-list/latest/List-Nonempty#Nonempty) `type Nonempty a = Nonempty a (List a)`; there will never be a reason to change the type's definition so the tag can be safely exported.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### exporting a union type and some of its tags

It is possible to only export *some* of the tags. However, there is no reason to do this, ever.

```elm
module MyModule exposing (Msg (Increment, Decrement))
type Msg = Increment | Decrement
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

#### exporting a union type 


Importing union types exposed follows the exact same syntax. For example, `import MyModule exposing (Msg)` will import only the type, while `import MyModule exposing (Msg(..))` will import any exported tags as well. You can also use exported tags qualified, like `MyModule.Increment`.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Modules,%20Exports,%20and%20Imports.md))

### Further Reading

- [Evan Czaplicki's discussion of union types](https://gist.github.com/evancz/06fe634245a3aab4a61b)
- [elm-explained](https://github.com/niksilver/elm-explained)
- [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html)


