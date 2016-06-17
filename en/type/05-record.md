**Elm 0.??**, **copied, please ignore** 

## Records 

Records (semantically named) are frequently employed to record state or to model information. They employ key-value pairs, with (likely-familar) dot notation for accessing values. (source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))

Records are written with brackets. A `record` is a collection of key/value pairs (name fields).

```elm
{name=value,...,name=value}
```

```elm
myRecord = { style = "Blue", number = 1, isCool = True }
```


Records values use equals between key and value but a colon between key and value type. 

```elm
point : {x : Float, y : Float}
point = {x = 3.2, y = 2.5}
```
-----
Records have fields with names. The order of fields doesn't matter. Notice that record values use equals signs, not colons.

```elm
{ x = 3, y = 7 }
```
-----
```elm
planarDistance : {a | x : Float, y : Float} -> {b | x : Float, y : Float} -> Float
planarDistance p1 p2 =
  let dx = p2.x - p1.x
      dy = p2.y - p1.y
  in sqrt (dx^2 + dy^2)
```

The `{a |` part of the annotation indicates a base record, of type `a`, is extended. Then we list the fields it's extended with, and what their types are. In the simplest cast, `a` can be the empty record, i.e. there are no extra fields. We use a different type variable, `b`, for the second argument to indicate that the two records don't have to be the same type. For example:

```elm
point3D = {x = 1.0, y = 6.3, z = -0.9}
dist = planarDistance point point3D
```
(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md) and [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

### Structural type

The type of a record is just the description of its fields. This next code says the type of `posA` is a record with a field `x` that's a Float and a field `y` that's a Float.

```elm
posA : { x : Float, y : Float }
posA = { x = 2.1, y = 8.8 }
```

The type of `posA` does not have a name, though.

(source: [elm-explained](https://github.com/niksilver/elm-explained))

### Creating

To create a record, you can write out the desired key-value pairs: { name = "Wuthering Heights", author = "Bronte", readByMe = True }. (Note that notation here may be backwards from what you expect. Type signatures, in Elm, use a colon. Assignment uses an equals sign)

#### Record type alias (Naming record types)

If I define `type alias Point2D = {x : Float, y : Float}`, then like any type alias `Point2D` becomes a valid type to use in annotations. But because we're aliasing a record, we also gain a *record constructor*, `Point2D : Float -> Float -> Point2D`. For example, `origin = Point2D 0 0` becomes legal, and this is actual Elm code, not an annotation. `Point2D` is both a type and a function.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md))

---

We can name a record type by tagging it - like this:

```elm
type Positioned = Positioned { x : Float, y : Float }

posB : Positioned
posB = Positioned { x = 7.7, y = 5.2 }
```

However, this isn't anything new. This is just tagging a record type in the same way we might tag an Int type, like this:

```elm
type Aged = Aged Int

century = Aged 100
```

By the way, `posA` and `posB` do not have the same type. `posA` is of type `{ x : Float, y : Float }` while `posB` is of type `Positioned`.


(source: [elm-explained](https://github.com/niksilver/elm-explained))
---
If we are planning to create more than one record with the same fields on it, we may want to create a type alias that will describe the shape of these records.

```elm
type alias LibraryEntry =
    { name : String
    , author : String
    , readByMe : Bool
    }
Creating a record with these fields looks like this:
wutheringHeights =
    { name = "Wuthering Heights"
    , author = "Bronte"
    , readByMe = True
    }
```

Now we can use LibraryEntry in type signatures, rather than writing out all three requisite fields. We can also use the LibraryEntry as a constructor. LibraryEntry takes three arguments, in order: name, author, and readByMe. It will hand us back a record of the form we desire.

```elm
wutheringHeights =
    LibraryEntry "Wuthering Heights" "Bronte" True

diaspora =
    LibraryEntry "Diaspora" "Egan" False
```    
As you can imagine, this pattern comes in very handy when there is a great deal of data to model, or when there are many records that you’d like to create that should all share the same type.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))


### Operations on records

#### Accessing

Use record.keyor .key record to access the fields of a record. 

----
Uniquely these functions are defined by a pattern, rather than being listed somewhere. For example, `.name : { b | name : a } -> a`, which basically means `.name` takes any record with a `name` field and extracts its value. You can use any record field name you like.

Beware of creating data structures with record accessors. Because all a list's elements must have the same type, each record accessor must extract a value of the same type, which is usually not what you want.
```elm
[.name, .number] : List ({ b | name : a, number : a } -> a)
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md) and [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))
-----
Access a field with a dot and the field name.

```elm
{ x = 3, y = 7 }.x -- 3
```

Or with an accessor fuction, which is a dot and the field name on its own.

```elm
.y { x = 3, y = 7 } -- 7
```
----
Accessing records
```elm
> myRecord.style
"Blue" : String
> .style myRecord
"Blue" : String
```
---
Getting and Setting on Records

Now we have wutheringHeights and diaspora, representing books in our library. But wait! I actually read Diaspora by Egan two weeks ago. We need to update the readByMe field.

```elm
{-

Updating records follows this pattern:

newRecordName =
    { oldRecordName | fieldName = newFieldValue }

-}

newDiaspora =
    { diaspora | readByMe = True }
```    

Note that diaspora is unchanged by this operation: diaspora.readByMe == False and newDiaspora.readByMe == True.

Dot notation is not the only way we can access a value on a record. For every field name on a record, we are given a corresponding accessor function named after the field. For example: .readByMe diaspora == False and .readByMe newDiaspora == True.

You will never have to write (\book -> book.name); you can use .name newDiaspora without defining anything additional.

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))


#### Immutable update

To copy a record but with some fields different, use
{ old_record | key1 <- new_value_1, ..., key_n <- new_value_n }

Update the fields of a record. (It must have the fields already.)

```elm
{ person | name = "George" }
```

Update multiple fields at once, using the current values.

```elm
{ particle |
  position = particle.position + particle.velocity,
  velocity = particle.velocity + particle.acceleration }
```

(source: ???)
---
Records are immutable. Updating records returns a new record
```elm
> updatedRecord = { myRecord | style = "Red", number = 10, isCool = False }
> myRecord.style
"Blue" : String
> updatedRecord.style
"Red" : String
```


#### Specifying only some fields


If the argument to a function is a record, you can specify which fields must be present. Example:

```elm
rec = {x = 5, y = 7, z = 3}
maxxz {x, z} = if x > z then x else z maxxz rec -- returns 5
```

(source: ???)

It is possible to write functions that work on records that have some of the fields but ignoring any other fields.


The type system allows you to ask for a certain thing to exist in a record.

    sendEmail : { record | email : String } -> Bool
    sendEmail userOrSomethingEmailable =
      -- code that sends email ...

This type annotation requires that we pass a record to the function that contains an email which must be a string. The rest of the fields in the record can be anything at all! This allows us to have a strong contract for this function but provides flexibility around the thing we pass into it.

(source: [understanding-the-elm-type-system.html](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

It is  possible to write functions that work on records as long as they have the right fields, ignoring any other fields.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

#### Pattern matching / destructuring


```elm
{ style, number, isCool } = myRecord
> style
"Blue" : String
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
---
```elm
myRecord = { x = 3, y = 4 }

sum {x,y} =
  x + y

sum myRecord
-- 7
```

Notice that the variable declared on the left side must match the key of record:

```elm
sum {a,b} =
  a + b

sum myRecord
-- The argument to function `sum` is causing a mismatch.
```

As long as our variable match one of the key of record, we can ignore other.

```elm
onlyX {x} =
  x

onlyX myRecord
-- 3 : number
```

(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))
----
##### Nested record

I don't think Elm support destructuring in nested record (I tried) because Elm encourages sparse record


```elm
format : ParseTree -> State -> State
format parseTree state =
  case parseTree of
    Node Feature (LeafNode (Description description) :: children) ->
      appendDesribe description children state

    Node Scenario (LeafNode (Description description) :: children) ->
      appendDesribe description children state

    Node Test (LeafNode (Description description) :: []) ->
      let
        open = tabs state ++ "it('" ++ description ++ "', function() {\n"
        close = tabs state ++ "});\n"
      in
        { state | output = state.output ++ open ++ close }

    _ ->
      state
```
(source: [yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

That one is very useful for record-like opaque types as they don't get the magical accessor functions, especially as you can do partial matching:

```elm
type AThing = AThing { foo: String, bar: Int }

foo (AThing { foo }) = foo
```
(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))

The latest version of Elm does support destructuring of nested types. Here's an example function I wrote to walk through an AST and return JavaScript code as a String.

```elm
format : ParseTree -> State -> State
format parseTree state =
  case parseTree of
    Node Feature (LeafNode (Description description) :: children) ->
      appendDesribe description children state

    Node Scenario (LeafNode (Description description) :: children) ->
      appendDesribe description children state

    Node Test (LeafNode (Description description) :: []) ->
      let
        open = tabs state ++ "it('" ++ description ++ "', function() {\n"
        close = tabs state ++ "});\n"
      in
        { state | output = state.output ++ open ++ close }

    _ ->
      state
```

(source: [comment on yang-wei gist](https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e))



### Extensible record types

#### Defining extensible records

We can define a record type that has *at least* the given fields. Here we define aliases for two record types. Type `JustXY` has just the fields `x` and `y`. It is an ordinary record type.
Type `IncXY` has at least the fields `x` and `y`. It is an extensible record type.

```elm
type alias JustXY  =     { x : Float, y : Float }

type alias IncXY a = { a | x : Float, y : Float }
```

Notice that the `IncXY` type has to declare the use of type variable `a`. That's because the type definition effectively reads "something of type `a` which also has a field `x` which is a Float and a field `y` which is a Float". See [our notes on parameterised types](ParameterisedTypes.md) elsewhere.

(source: [elm-explained](https://github.com/niksilver/elm-explained))


#### Defining values with extensible record types

Here we define a value of type `JustXY` and one of type `IncXY`.

```elm
jxy : JustXY
jxy = { x = 1.1, y = 2.1 }

ixy : IncXY { z : Float }
ixy = { x = 10.1, y = 11.1, z = 12.1 }
```

Notice the very different type declaration of `ixy`. This is because `IncXY` was declared with a variable type (`IncXY a`). Now that we're actually using it we must state what that variable type is (i.e. what the `a` is) in this particular case. And in this case it's the `z` part of the record.

Now we define two functions which take a `JustXY` and an `IncXY`. The `sumInc` function will accept values of both types, but the `sumJust` function will only take a value of `JustXY`.

```elm
sumJust : JustXY -> Float
sumJust coords =
    coords.x + coords.y

sumInc : IncXY a -> Float
sumInc coords =
    coords.x + coords.y
```

Here's how we can (and can't) use them:

```elm
res1 = sumJust jxy
-- This next line won't compile if we uncomment it
-- res2 = sumJust ixy
res3 = sumInc ixy
res4 = sumInc jxy
```

If we put all the code above into [an Elm module called ExtensibleRecordTypes](ExtensibleRecordTypes.elm) then this is what it looks like in the Elm REPL:

```
> import ExtensibleRecordTypes exposing (..)
> ixy
{ x = 10.1, y = 11.1, z = 12.1 } : ExtensibleRecordTypes.IncXY { z : Float }
> jxy
{ x = 1.1, y = 2.1 } : ExtensibleRecordTypes.JustXY
> res1
3.2 : Float
> res3
21.2 : Float
> res4
3.2 : Float
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

## -- 2 sort --

### No Extensibility

The first iteration of the Elm record was based heavily on (‘Extensible records with scoped labels’ by Daan Leijen), and many features of the Elm record reflect Leijen’s work. The paper is well worth a read for those interested in modeling data. However, the paper’s titular descriptors–“extensible”, “with scoped labels”–no longer apply to Elm records.

Extensibility (which was paired with restrictability) of a record value meant that a record could have fields added to it and removed from it: e.g., { wutheringHeights - name } == { author = Bronte, readByMe = True }. In practice, developers did not find this feature useful, and extensibility of record values was removed.

The removal of the extensibility of records also meant the end to scoped labels (labels of the same name pointing to different values on the same record, e.g.
```elm
{ diaspora + name | name {- This type alias uses "a" to say that a LibraryEntry is any record with name, author, and readByMe fields. -} type alias LibraryEntry a = { a | name : String , author : String , readByMe : Bool }`

`{- This type alias says that it is a LibraryEntry with exactly one more property–isbn. -} type alias LibraryEntryWithISBN = LibraryEntry { isbn : String }`

`wutheringHeightsWithISBN : LibraryEntryWithISBN wutheringHeightsWithISBN = { name = “Wuthering Heights” , author = “Bronte” , readByMe = True , isbn = “1853260010” }
```

Records in Elm remain polymorphic, meaning they can be comprised of fields with all sorts of different types. While I’ve only used Strings and Bools in the library example above, records can hold any values. The following is reasonable, for instance:

```elm
type alias Widget entry =
    { text : entry -> Html
    , attributes : entry -> Html
    , getState : entry -> Bool
    }
```

Records are also transitive with respect to their fields. That is, defining:

```elm
oneBeforeTwo =
    { one = 1
    , two = 2
    }

twoBeforeOne =
    { two = 2
    , one = 1
    }
```    

We will find that oneBeforeTwo and twoBeforeOne are equal, even though the fields are declared in different orders. If, though, in twoBeforeOne our field values were strings instead of numbers, we’d find that the compiler would not let us make the comparison at all: we’d be causing a type mismatch.

This is because Records in Elm are structurally typed: the use of records is dependent on the type of their contents. We can’t just compare Records with each other because they’re both Records–we can compare Records if all their fields have the same types.

When we create functions consuming Records, they can be as general or as specific as we please. I can write a getName function that takes any Record with a name field, or I can write a displayHeader function that requires name, author, publishDate, editDate, yourFirstBorn, and maybe a list of related titles too, just because.

```elm
getSentenceForLibraryEntry : LibraryEntry -> String
getSentenceForLibraryEntry {name, author} = -- note that Elm gives us clean desctructuring
    author ++ " wrote " ++ name ++ "."


getSentenceForLibraryEntry : LibraryEntry -> String
getSentenceForLibraryEntry libraryEntry =
    libraryEntry.author ++ " wrote " ++ libraryEntry.name ++ "."
```

(source: [Data Structures in Elm @NoRedInk](http://tech.noredink.com/post/140646140878/data-structures-in-elm))


## Type aliases for records act as multi-parameter functions

If we have a type alias for a record then that also acts as a function, but in a different way from a type declaration. Let's define a type alias...

```elm
type alias Person = { name : String, age : Int }
```

Now the `Person` function is one that takes several arguments, one for each field, and it will return a record with those fields. This means we need to know the order in which we originally defined the fields.

If we look at it in the REPL we see that `Person` is indeed a function:

```
> import DefiningFunctions exposing (..)
> Person
<function> : String -> Int -> DefiningFunctions.Person
>
```

Now let's define a value using the `Person` function, passing in two arguments:

```elm
bob : { name : String, age : Int }
bob = Person "Robert" 55
```

And here we use the `Person` function to bring a list of names and a list of ages together:

```elm
people : List { name : String, age : Int }
people =
    map2 Person ["Alice", "Brian", "Coco"] [61, 23, 35]
```

Let's see what that looks like in the REPL:

```
> import DefiningFunctions exposing (..)
> bob
{ name = "Robert", age = 55 } : { age : Int, name : String }
> people
[{ name = "Alice", age = 61 },{ name = "Brian", age = 23 },{ name = "Coco", age = 35 }]
: List { age : Int, name : String }
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))


## Record field names define functions

Any field name is a function if you precede it immediately with a dot (no whitespace). It is a function that takes any type of record with that field, and returns the value of that field. Here's what we can do in the Elm REPL:

```
> .name
<function> : { b | name : a } -> a
> .age
<function> : { b | age : a } -> a
> .totallyUnlikelyMadeUpField
<function> : { b | totallyUnlikelyMadeUpField : a } -> a
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

## Type tags for records are also functions

Next we define a type for a record called `Point`, and its associated tag/constructor (also called `Point`). But that tag is also a function from the plain record to a `Point`. This is exactly the same idea as with simple type tags. We can see this when we use it with the `map` function.

```elm
type Point = Point { x : Float, y : Float }

triangle1 : List { x : Float, y : Float }
triangle1 =
    [ { x = 0, y = 0 }
    , { x = 8, y = 1 }
    , { x = 5, y = 7 }
    ]

triangle2 : List Point
triangle2 =
    map Point triangle1
```

Here's how it looks in the REPL. Notice that we've converted from the type of `triangle1` to the type of `triangle2` thanks to the `Point` function.

```
> import DefiningFunctions exposing (..)
> triangle1
[{ x = 0, y = 0 },{ x = 8, y = 1 },{ x = 5, y = 7 }]
    : List { x : Float, y : Float }
> triangle2
[Point { x = 0, y = 0 },Point { x = 8, y = 1 },Point { x = 5, y = 7 }]
    : List DefiningFunctions.Point
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

#### All operations

[records](http://elm-lang.org/docs/syntax#records) has a number of operations without much explanation. 
