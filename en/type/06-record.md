**Elm 0.17**, **shareable**

## Records

Records offer named fields. They allow you to associate values with keys in an efficient manner. No duplicate keys are allowed.

Records are written with brackets. The brackets enclose a collection of key-value pairs, or semantically named fields `myRecord = { name1 = value1, name2 = value2 }`. Records in Elm are polymorphic, meaning they can be comprised of fields with all sorts of different types. They frequently employed to record state or to model information.

```elm
unidentifiedPerson d
```

### Creating a record

#### key-value pairs

The typical way to create a record is to write key-value pairs

```elm
myRecord = { first = "Jane", last = "Doe", age = 42, gender = "female" }
```

### Manipulating Records

#### accessing

Use `record.key` or `.key record` to access the fields of a record.

```elm
$ elm repl
> { x = 3, y = 7 }.x
3 : number
> .y { x = 3, y = 7 }
7 : number
```

Uniquely these functions are defined by a pattern. Any field name is a function if you precede it immediately with a dot (no whitespace).

```elm
$ elm repl
> .x
<function> : { b | x : a } -> a
> .aPatternThanElmKnowsNothingAbout
<function> : { b | aPatternThanElmKnowsNothingAbout : a } -> a
```

#### updating, the immutable way

Updating records follows this pattern: `newRecordName = { oldRecordName | fieldName = newFieldValue }`. The fields must exist.

```elm
$ elm repl
> unidentifiedPerson = { first = "Jane", last = "Doe", age = 42, gender = "female" }
{ first = "Jane", last = "Doe", age = 42, gender = "female" }
    : { age : number, first : String, gender : String, last : String }
> identifiedPerson  = { myRecord | first = "Erica", last = "Urban" }
{ first = "Erica", last = "Urban", age = 42, gender = "female" }
    : { age : number, gender : String, first : String, last : String }
```

Because records are immutable, we return a new record.

#### functions that operate only on some of the fields

If the argument to a function is a record, you can specify which fields must be present. Example:

```elm
$ elm repl
> person = { first = "Jane", last = "Doe", age = 42, gender = "female" }
{ first = "Jane", last = "Doe", age = 42, gender = "female" }
    : { age : number, first : String, gender : String, last : String }
> printLastName {last} = last
<function> : { b | last : a } -> a
> printLastName person
"Doe" : String
```

Alternatively, in you can ask in the signature for a certain field to exist in a record. The rest of the fields can be anything.

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = div []
  [ text ( toString ( planarDistance {x = 1, y = 10}  {x = 1, y = 6.3} ) )
  , br [] []
  , text ( toString ( planarDistance {x = 1, y = 10}  {x = 1.0, y = 6.3, z = -0.9} ) )
  ]
-- 3.7
-- 3.7

planarDistance : {a | x : Float, y : Float} -> {b | x : Float, y : Float} -> Float
planarDistance p1 p2 =
  let dx = p2.x - p1.x
      dy = p2.y - p1.y
  in sqrt (dx^2 + dy^2)
```

The `{a |` part of the annotation corresponds to a record extension. See above. We introduce two type variables, `a` and `b` to indicate that the two records don't have to be the same type.

#### Pattern matching / destructuring

```elm
$ elm repl
> isAtOrigin {x,y} = \
  case (x,y) of \
    (0,0) -> True \
    (_,_) -> False
> isAtOrigin { x = 3, y = 4 }
False : Bool
> isAtOrigin { x = 0, y = 0, z = 3 }
True : Bool
```

#### nested record


The latest version of Elm does support destructuring of nested types.  (source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))


### All operations

[records](http://elm-lang.org/docs/syntax#records) has a number of operations without much explanation.


----------------------------------
### Type annotations

For value assignment, records use equals between key and value when specifying values. However, in the type signature, records use  a colon between key and value type.

```elm
$ elm repl
> myRecord = { first = "Jane", last = "Doe", age = 42, gender = "female" }
{ first = "Jane", last = "Doe", age = 42, gender = "female" }
    : { age : number, first : String, gender : String, last : String }
```

In a program

```elm
$ open http://elm-lang.org/try
import Html exposing (text)
main = text ( toString userProfile )
userProfile : {first : String, last : String, age: Int, gender: String}
userProfile = { first = "Jane", last = "Doe", age = 42, gender = "female" }
```

#### structural type, field order doesn't matter

Records in Elm are structurally typed. The type of a record is just the description of its fields: the names given of the fields and the type of their contents.

Records are transitive with respect to their fields. The order of fields doesn't matter.

```elm
$ elm
> { one = 1 , two = 2 } == { two = 2 , one = 1 }
True : Bool
> { one = 1 , two = 2 } == { two = 3 , one = 1 }
False : Bool
```

However, we can compare Records only when their types are equivalent.

```elm
$ elm
> { one = 1 , two = 2 } == { one = 1 , two = 2 }
True : Bool
> { one = 1 , two = 2 } == { one = 1 , two = "2" }
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
> { one = 1 , two = 2 } == { one = 1 , otherThanTwo = 2 }
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
> { one = 1 , two = 2 } == { one = 1 , two = 2, three = 3 }
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
```

#### aliasing record types

If we are planning to create more than one record with the same fields on it, we can create a type alias that describes the shape of these records.

```elm
$ elm repl
> type alias Point2D = {x : Float, y : Float}
```

This creates a *record constructor* that is both a function and a type.

```elm
> type alias Point2D = {x : Float, y : Float}
> Point2D
<function> : Float -> Float -> Repl.Point2D
> [(Point2D 0 0), (Point2D 0 1), (Point2D 1 1), (Point2D 1 0)]
[{ x = 0, y = 0 },{ x = 0, y = 1 },{ x = 1, y = 1 },{ x = 1, y = 0 }]
    : List Repl.Point2D
```

The constructor acts as multi-parameter functions to construct records. It takes one argument for each field, and returns a record with those fields

```elm
$ elm repl
> Point2D
> origin = Point2D 0 0
{ x = 0, y = 0 } : Repl.Point2D
> line2D = List.map (\pt -> Point2D pt.x pt.y) [ { x = 0, y = 0 }, { x = 8, y = 1 } ]
[{ x = 0, y = 0 },{ x = 8, y = 1 }] : List Repl.Point2D
```

In this context, field order does matter. We need to know the order in which we originally defined the fields

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = div []
  [ text ( toString ( Person "Jane" 42 ) )
  , br [] []
  , text ( toString ( Person2 42 "Jane" ) )
  ]
-- { name = "Jane", age = 42 }
-- { age = 42, name = "Jane" }
type alias Person = { name : String, age : Int }
type alias Person2 = { age : Int, name : String }
```

The alias can be used in type signatures, in place of all required fields.

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = text ( toString ( midPoint (Point2D 1 10)  (Point2D 5 7) ) )
-- { x = 3, y = 8.5 }
type alias Point2D = {x : Float, y : Float}
midPoint : Point2D -> Point2D -> Point2D
midPoint p1 p2 = Point2D ((p1.x + p2.x) / 2) ((p1.y + p2.y) / 2)
```

See [Type Constructor](11-type constructor.md)

Record type aliases are very useful in html application. They let you define core models for your view. Consider the following example:

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

Using a type alias we can describe the exact shape of what a user record should be. Our Elm app won’t compile if a user record is missing one of these entries, has an extra entry, or an entry is not the correct type.

If you alias a record, you can use the name as a constructor function.

```elm
> type alias User = { name : String , email : String , age : Int, admin : Bool }
> User "john" "john@somewhere.com" 42 True
{ name = "john", email = "john@somewhere.com", age = 42, admin = True }
    : Repl.User
```

#### extending another record type

We can define an extensible record type by specifying the fields that must *at least* be present. The syntax is `type alias BaseRecord a = { a | key1 : Type1 , key2 : Type2}` and reads "something of type `a` which also has a field `key1` which is of `Type1` and a field `key2` which is of `Type2`".

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = div []
  [ text (toString origin2D)
  , br [] []
  , text (toString origin3D)
  ]
-- { x = 0, y = 0 }
-- { x = 0, y = 0, z = 0 }
-- origin2D = { x = 0, y = 0, z = 0} will show an error
-- origin3D = { x = 0, y = 0} will show an error

type alias Point a = { a | x : Float , y : Float}

type alias Point2D = Point {}
origin2D : Point2D
origin2D = { x = 0, y = 0}

type alias Point3D = Point { z : Float }
origin3D : Point3D
origin3D = { x = 0, y = 0, z = 0 }
```

The `{a |` part of the annotation indicates a base record, of type `a`, is extended. Then we list the fields it's extended with, and what their types are. In the simplest cast, `a` can be the empty record, i.e. there are no extra fields.

Note that extensible types have no constructor function attached.

```elm
> type alias Point a = { a | x : Float , y : Float}
> Point
-- NAMING ERROR ---------------------------------------------- repl-temp-000.elm
Cannot find variable `Point`
> type alias Point3D = Point { z : Float }
> Point3D
-- NAMING ERROR ---------------------------------------------- repl-temp-000.elm
Cannot find variable `Point3D`
```

Or in the program provided before.

```elm
$ open http://elm-lang.org/try
import Html exposing (div, text, br)
main = div []
  [ text (toString ( midJustX { x = 1, y = 10} { x = 3, y = 7}) )
  , br [] []
  , text (toString ( midJustX (PointJust 1 10) (PointJust 3 7) ) )
  , br [] []
  , text (toString ( midExtX { x = 1, y = 10} { x = 3, y = 7}) )
  , br [] []
  , text (toString ( midExtX (PointJust 1 10) (PointJust 3 7) ) )
  , br [] []
  , text (toString ( midExtX { x = 1, y = 10, z = 4} { x = 3, y = 7, z = 8}) )
  , br [] []
  , text (toString ( midExtX fst3D { x = 3, y = 7, z = 8}) )
  ]
-- 2
-- Point2D 0 0 --> Cannot find variable `Point2D`, Point2D is a type, not a constructor
-- midJustX { x = 1, y = 10, z = 4} { x = 3, y = 7, z = 8}) --> TYPE MISMATCH

type alias PointExt a = { a | x : Float , y : Float}
type alias PointJust = { x : Float , y : Float}

type alias Point2D = PointExt {}

midExtX : PointExt a -> PointExt a -> Float
midExtX p1 p2 = ((p1.x + p2.x) / 2)

midJustX : PointJust -> PointJust -> Float
midJustX p1 p2 = ((p1.x + p2.x) / 2)

fst3D : PointExt { z : Float }
fst3D = { x = 1, y = 10, z = 4}
```



### Further Readings

* [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm)
* [elm-explained](https://github.com/niksilver/elm-explained)
* [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md)
* [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md)
* [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e)


The first iteration of the Elm record was based heavily on (‘Extensible records with scoped labels’ by Daan Leijen), and many features of the Elm record reflect Leijen’s work. The paper is well worth a read for those interested in modeling data. However, the paper’s titular descriptors–“extensible”, “with scoped labels”–no longer apply to Elm records. [I]n practice, developers did not find Extensibility (or restrictability) useful, and extensibility of record values was removed. (source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))
