**Elm 0.17**, **copied, please ignore** 

## Type constructors

*Type constructors* are base types for a new type shape. In `List a`, the type variable `a` can be replaced with a real type. They cannot really exist on its own. 

### simple tag constructor

If we've defined a type with a tag then the tag (the constructor) is a function, too.

Here we use `map` to convert a list of Int values into a list of `Boxed` values.

```elm
type Boxed = Boxed Int

box : List Int -> List Boxed
box li = map Boxed li
```

Remember that `DefiningFunctions` is just the name of our module, so `DefiningFunctions.Boxed` is just the fully qualified name of `Boxed`:

```
> import DefiningFunctions exposing (..)
> box [7, 6, 5, 4]
[Boxed 7,Boxed 6,Boxed 5,Boxed 4] : List DefiningFunctions.Boxed
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

Tagging an Int type, like this:

```elm
type Aged = Aged Int
century = Aged 100
```


This only makes sense when there is a single tag.

```elm
type Weight = Weight Int
star1 : Weight
star1 = Weight 100
```


### record tag constructor

Record types can be named by tagging them.  See [Records](05-record.md)

```elm
pos A = { x = 7.7, y = 5.2 }
type Positioned = Positioned { x : Float, y : Float }

posB : Positioned
posB = Positioned { x = 7.7, y = 5.2 }
```

Note that `posA` and `posB` do not have the same type. `posA` is of type `{ x : Float, y : Float }` while `posB` is of type `Positioned`.

(source: [elm-explained](https://github.com/niksilver/elm-explained))

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br, hr, h3)
main = div [] 
  [ text ( launch vessel )
  , br [] []
  , text ( greet captain )
  -- , br [] []
  -- , text ( launch captain ) -- > TYPE MISMATCH
  -- , br [] []
  -- , text ( greet vessel ) -- > TYPE MISMATCH
  ]
{-
I name this ship HMS Splendid
Hello there, Captain Robert Q. Peabody
-} 

type Ship = Ship { name : String }
type Person = Person { name : String }

vessel : Ship
vessel = Ship {name = "HMS Splendid"}

captain : Person
captain = Person {name = "Captain Robert Q. Peabody"}

launch : Ship -> String
launch s
    = case s of
        Ship r -> "I name this ship " ++ r.name

greet : Person -> String
greet p
    = case p of
        Person r -> "Hello there, " ++ r.name
```

### Offers better type protection than type alias

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br, hr, h3)
main = div [] 
  [ text ( launchAlias vesselAlias )
  , br [] []
  , text ( greetAlias captainAlias )
  , br [] []
  , text "(no type protection)"
  , br [] []
  , text ( launchAlias captainAlias )
  , br [] []
  , text ( greetAlias vesselAlias )
  ]
{-
I name this ship HMS Splendid
Hello there, Captain Robert Q. Peabody
(no type protection)
I name this ship Captain Robert Q. Peabody
Hello there, HMS Splendid
-} 

type alias ShipAlias = { name : String }
type alias PersonAlias = { name : String }

vesselAlias : ShipAlias
vesselAlias = ShipAlias "HMS Splendid"

captainAlias : PersonAlias
captainAlias = PersonAlias "Captain Robert Q. Peabody"

launchAlias : ShipAlias -> String
launchAlias s
    = case s of
        r -> "I name this ship " ++ r.name

greetAlias : PersonAlias -> String
greetAlias p
    = case p of
        r -> "Hello there, " ++ r.name
```
(source: [elm-explained](https://github.com/niksilver/elm-explained))


### can take multiple arguments

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br, hr, h3)
main = div [] 
  [ text ( describe n37 )
  ]
{-
Number thirty seven looks like 37
-} 

type NamedInt = NamedInt String Int

describe : NamedInt -> String
describe ni
  = case ni of
      NamedInt name num -> "Number " ++ name ++ " looks like " ++ (toString num)

n1 = NamedInt "one" 1
n2 = NamedInt "two" 2
n37 = NamedInt "thirty seven" 37
```
(source: [elm-explained](https://github.com/niksilver/elm-explained))

#### Parametrised types



When we declare a type, the left had side of the equals sign says how it's used; the right hand side says how it's defined. The following `MiscHolder` can hold any type of value. It's defined as an unknown type `a` tagged with the `MiscHolder` tag (its right hand side), but when we use it we must use both the `MiscHolder` tag and the specific type that's in use at the time.

```elm
type MiscHolder a = MiscHolder a
```

Here's how we use it to define some values, each with its own type declaration.

```elm
stringInABox : MiscHolder String
stringInABox = MiscHolder "Greetings, Earthlings"

floatInABox : MiscHolder Float
floatInABox = MiscHolder 45.1
```

Since `MiscHolder` has only one additional type, when we use it to hold a List of Strings we have to put the `List String` in parentheses, otherwise the compiler will report an error.

```elm
listOfStringsInABox : MiscHolder (List String)
listOfStringsInABox = MiscHolder ["Once", "upon", "a", "time"]
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

#### Container Type

With collections like lists, tuples, records, annotations take a slightly different form. They specify both the type of the collection and the type of value held in the collection. 

List:
```elm
flagColors: List String
flagColors = ["Black", "Yellow", "Red"]
diceFaces : List Int
diceFaces = [1,2,3,4,5,6]
```

Tuple:
```elm
answerToEverything : (String, Int, Bool)
answerToEverything = ("the answer", 42, True)
```

Record:
```elm
startPosition : { x : Float, y : Float }
startPosition =
    { x = 0,
      y = 0
    }    
```


### A more involved example

Let's define a type for something that's been weighed. We need to capture the thing itself, the weight as a floating point number, and the weighing units (expressed as a String). When we use `Weighed` in a type declaration in future we need to use its name and the type of thing that's been weighed.

First, some type definitions to get us started.

```elm
type Car = Car String
type AnimalPart = AnimalPart String
```

Now the main type definition.

```elm
type Weighed a = Weighed a Float String
```

Notice that the thing being weighed (which has unspecified type `a`) has to have its type exposed on the left hand side of the equals sign. Then when we declare the type of a `Weighed` thing we need to specify the thing's type, too (a `Car` or an `AnimalPart`).

Now let's see some simple use...

```elm
volvo = Car "Volvo"
feather = AnimalPart "Ostrich feather"

volvoWeighed : Weighed Car
volvoWeighed = Weighed volvo 2260 "kg"

featherWeighed : Weighed AnimalPart
featherWeighed = Weighed feather 8.5 "g"
```

We can do it without the interim values, too, but if so we have to use parentheses...

```elm
volvoWeighed2 : Weighed Car
volvoWeighed2 = Weighed (Car "Volvo") 2260 "kg"

featherWeighed2 : Weighed AnimalPart
featherWeighed2 = Weighed (AnimalPart "Ostrich feather") 8.5 "g"
```

If we were to put the code above into [an Elm module called ParameterisedTypes](ParameterisedTypes.elm) then here's how we might explore it in the Elm REPL:

```
> import ParameterisedTypes exposing (..)
> volvoWeighed
Weighed (Car "Volvo") 2260 "kg"
    : ParameterisedTypes.Weighed ParameterisedTypes.Car
> volvoWeighed == volvoWeighed2
True : Bool
> featherWeighed == featherWeighed2
True : Bool
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))




