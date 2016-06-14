**Elm 0.??**, **copied, please ignore** 

# Type Aliases

```elm
type alias Description = String

description : Description
description = "abc"
```

```elm
type alias Point = { x : Float, y : Float }

origin : Point
origin =
  { x = 0, y = 0 }
```

Type aliases help you model the problem

Type aliases allow you to attach human readable names to existing types. This helps you model your problem and reason about how data flows through your program. A simple example is:

```elm
type alias Email = String
```

With this type alias in place, we can rewrite the above type annotation as follows:

```elm
sendEmail : { record | email : Email } -> Bool
```

This becomes a very powerful tool when applied to records as it allows you to define your core models for your view. Consider the following example:

```elm
type alias User =
  { name : String
  , email : Email
  , age : Int
  , admin : Bool
  }

  userView : User -> Html
  userView user =
    -- code that renders a view for user ...
```

Using a type alias we can describe the exact shape of what a user record should be. Our Elm app won’t compile if a user record is missing one of these entries, has an extra entry, or an entry is not the correct type. This ensures that any bugs are caught long before a customer ever sees them.

As you program in Elm, you follow a delicious breadcrumb trail of extremely readable compiler error messages until the program compiles and everything works. By leveraging the type system in Elm, you account for all scenarios before you even open a browser. You never open the JavaScript console.

For example, when writing an Elm program I might at some point decide that users should have an admin flag. I will then try to use that flag in a function at which point the compiler will tell me that I have failed to add it to the User model. I will add it to the model at which point the compiler will tell me that I have failed to account for it in my main update function.

You really have to try Elm yourself to get a feel for the power of this idea and the way it guides your programming style.

(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])

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

(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])


# Type aliases

A type alias doesn't define a type, it simply provides a convenient
alternative name for another type or record shape.

We will define a Ship and a Person, but the compiler sees them
as the same thing, which is a record with a name field.

```elm
type alias Ship = { name : String }
type alias Person = { name : String }
```

We'll create a Ship and a Person, but in fact they are both just
a record with a name field. The compiler only sees them as this.

```elm
vessel : Ship
vessel = { name = "HMS Splendid" }

captain : Person
captain = { name = "Captain Robert Q. Peabody" }
```

Here are two functions which seem to take a Ship and a Person as
arguments, but in fact they are really just taking a record with
a name field.

```elm
launch : Ship -> String
launch s
  = "I name this ship " ++ s.name

greet : Person -> String
greet p
  = "Hello there, " ++ p.name
```

Here we launch a ship and greet a person. As you'd expect, both
functions compile fine and give the expected output.

```elm
launchShip = launch vessel
greetPerson = greet captain
```

But these next two functions also compile and execute okay.
That's because Ship and Person are not types, they're just aliases
for a record with a name field.

```elm
launchPerson = launch captain
greetShip = greet vessel
```

We can copy all the code above into
[an Elm module called TypeAliases.elm](TypeAliases.elm).
And then here's what it looks like when we use the functions in the REPL:

```
> import TypeAliases exposing (..)
> launchShip
"I name this ship HMS Splendid" : String
> greetPerson
"Hello there, Captain Robert Q. Peabody" : String
> launchPerson
"I name this ship Captain Robert Q. Peabody" : String
> greetShip
"Hello there, HMS Splendid" : String
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

## Type alias

You can also define types at the top level, like `type alias Model = Int`.

The first is a record type alias. If I define `type alias Point2D = {x : Float, y : Float}`, then like any type alias `Point2D` becomes a valid type to use in annotations. But because we're aliasing a record, we also gain a *record constructor*, `Point2D : Float -> Float -> Point2D`. For example, `origin = Point2D 0 0` becomes legal, and this is actual Elm code, not an annotation. `Point2D` is both a type and a function.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

## Type Aliases

When you write a record or tuple, its type already exists.
(Notice that record types use colon and record values use equals.)
~~~~ {.Elm:hs name="code"}
origin : { x : Float, y : Float, z : Float }
origin =
  { x = 0, y = 0, z = 0 }
~~~~

You can give existing types a nice name with a type alias.
~~~~ {.Elm:hs name="code"}
type alias Point3D =
  { x : Float, y : Float, z : Float }
~~~~

If you alias a record, you can use the name as a constructor function.

~~~~ {.Elm:hs name="code"}
otherOrigin : Point3D
otherOrigin =
  Point3D 0 0 0
~~~~

But it's still the same type, so you can equate them.

~~~~ {.Elm:hs name="code"}
origin == otherOrigin -- True
~~~~

(source: ???)

## Type Aliases

You can give existing types a custom name with `type alias`

```elm
type alias Name = String
type alias Dob = String

type alias Record = { name: Name, dob: Dob }
```

We can use it later annotating function
```elm
record : Record
record =
    { name = "Dave", dob = "27/08/1999 }
```

But still `type alias` equals to it's parent `type`
```elm
type alias Name = String

name : Name
name =
  "Dave"

secondName : String
secondName =
  "Dave"

-- True
name == secondName
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
