**Elm 0.??**, **copied, please ignore** 

## Type constructors

Note. `List` is called a *type constructor*. However, It can't really exist on its own.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

If `List a` is a list of any type, what is just `List`? Technically it's called a *type constructor*, but the better answer is that it's not really anything. It can't really exist on its own. The best way to think of it is that `List a` is the base type, and sometimes the type variable `a` gets replaced with a real type.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))


A type constructor defines a new type, and gives us protection
against misuse.

In this example we'll define a Ship and a Person, which both have
a name, but we won't be able to substitute one for the other
in a function which requires a specific one.

```elm
type Ship = Ship String
type Person = Person String
```

The symbols Ship and Person are also called tags - they are tagging
the String to indentify intended use.

Next we'll create a specific Ship and Person.
They will therefore be of distinct types:

```elm
vessel : Ship
vessel = Ship "HMS Splendid"

captain : Person
captain = Person "Captain Robert Q. Peabody"
```

Here are two functions which take a Ship and a Person as arguments.
To extract the data embedded in the type we use pattern matching.

```elm
launch : Ship -> String
launch s
    = case s of
        Ship name -> "I name this ship " ++ name

greet : Person -> String
greet p
    = case p of
        Person name -> "Hello there, " ++ name
```

Here we launch a ship and greet a person. As you'd expect, both
functions compile fine and give the expected output.

```elm
launchShip = launch vessel
greetPerson = greet captain
```

If we put all the code above into
[an Elm module called TypeConstructors.elm](TypeConstructors.elm)
then We can execute it in the Elm REPL to check

```
> import TypeConstructors exposing (..)
> launchShip
"I name this ship HMS Splendid" : String
> greetPerson
"Hello there, Captain Robert Q. Peabody" : String
>
```

But these next two functions won't compile.

```elm
launchPerson = launch captain
greetShip = greet vessel
```

Here's what it looks like in the REPL if we try to add it into
our module and then import that module:

```
> import TypeConstructors exposing (..)
-- TYPE MISMATCH --------------------------------- .\.\TypeConstructors.elm

The argument to function `launch` is causing a mismatch.

57|                launch captain
                          ^^^^^^^
Function `launch` is expecting the argument to be:

    Ship

But it is:

    Person

>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))


## Example with more than one argument

We can of course also define a type with more than one argument.
Here is how we define and use a number with a name

```elm
type NamedInt = NamedInt String Int

describe : NamedInt -> String
describe ni
  = case ni of
      NamedInt name num -> "Number " ++ name ++ " looks like " ++ (toString num)

n1 = NamedInt "one" 1
n2 = NamedInt "two" 2
n37 = NamedInt "thirty seven" 37
```

If we add that code to our module
we can use it like this in the Elm REPL:

```
> import TypeConstructors exposing (..)
> describe n37
"Number thirty seven looks like 37" : String
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

### Type declaration ???

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
