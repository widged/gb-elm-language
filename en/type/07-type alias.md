**Elm 0.??**, **copied, please ignore** 

## Type Aliases

A type alias doesn't define a type, it simply provides a convenient alternative name for another type (primitive or complex shape). It helps you model the problem and reason about how data flows through your program.

```elm
type alias Point = { x : Float, y : Float }
origin : Point
origin = { x = 0, y = 0 }
```


### Type alias

You can also define types at the top level, like `type alias Model = Int`.

The first is a record type alias. If I define `type alias Point2D = {x : Float, y : Float}`, then like any type alias `Point2D` becomes a valid type to use in annotations. But because we're aliasing a record, we also gain a *record constructor*, `Point2D : Float -> Float -> Point2D`. For example, `origin = Point2D 0 0` becomes legal, and this is actual Elm code, not an annotation. `Point2D` is both a type and a function.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

### Type Aliases

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

### Type Aliases

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

#### on records

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

The compiler sees Ship and Person as the same thing, which is a record with a name field. However, if we later change the shape of Ship, by adding an extra field, they will stop being interchangeable. 

```elm
iimport Html exposing (div, text, br)

main = div [] 
  [ text (launch vessel)
  , br [] []
  , text (greet captain)
  , br [] []
  -- these two function also work, for as long as Ship and Person 
  -- have the exact same record shape. Add an extra field name to 
  -- either Ship or Person and you will get a TYPE MISMATCH
  , text (launch captain)
  , br [] []
  , text (greet vessel)
  ]
-- I name this ship HMS Splendid
-- Hello there, Captain Robert Q. Peabody
-- I name this ship Captain Robert Q. Peabody
-- Hello there, HMS Splendid

type alias Ship = { name : String }
type alias Person = { name : String }

vessel : Ship
vessel = { name = "HMS Splendid" }

captain : Person
captain = { name = "Captain Robert Q. Peabody" }

launch : Ship -> String
launch s
  = "I name this ship " ++ s.name

greet : Person -> String
greet p
  = "Hello there, " ++ p.name
```

### Further Readings

* [understanding-the-elm-type-system.html](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html)
