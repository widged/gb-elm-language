**Elm 0.??**, **copied, please ignore** 

# Scope

## Variable definitions

In Elm as in JavaScript, **scope** refers to what is defined, and where. It is the answer to the question, "where did this thing come from?", which you will ask constantly when reading examples or someone else's code. Scope also can tell
you why something *isn't* available, what code defines similar values, and where you can find documentation.

Scope in Elm is often simpler than in other languages because ordinary values do not change over time. Not counting syntax (e.g. `if`, `->`, and brackets), pretty much everything in Elm is either a literal, something you imported, or something you defined.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md))

### Top-Level Definitions

```elm
answer = 42
```

Elm is immutable, the variable cannot be set to another value in another part of the file. 


### Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. In this example, some values are function arguments, some are defined in the `let`, and some (the math operators) are imported automatically from Basics. (And `2` is a literal.)

```elm
distanceFrom (originX, originY) (x, y) =
    let dx = x - originX
        dy = y - originY
    in sqrt (dx^2 + dy^2)
```

After the `let`, you can place as many definitions as you like, just like at the top level. They can be fixed values or functions. You can even write type annotations, although you can't define new types.

The expression after the `in`, where all the definitions are in scope, is what the entire `let` expression becomes. Actually, the definitions are in scope even as you write more definitions. Here's a somewhat contrived example.

```elm
radToDeg rad =
    let piInDegrees = 180
        conversionFactor = piInDegrees/pi
    in conversionFactor * rad
```

Be aware that if you define the same name multiple times, the innermost definition is used. Usually you should just avoid the issue entirely by using unique names.

```elm
foo = 0

silly foo =
  let foo = 12
  in foo

silly 5 == 12 -- True
```

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Scope.md]])


## Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from
anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. In this example, some values are
function arguments, some are defined in the `let`, and some (the math operators) are imported automatically from Basics.
(And `2` is a literal.)

```elm
distanceFrom (originX, originY) (x, y) =
    let dx = x - originX
        dy = y - originY
    in sqrt (dx^2 + dy^2)
```

After the `let`, you can place as many definitions as you like, just like at the top level. They can be fixed values or
functions. You can even write type annotations, although you can't define new types.

The expression after the `in`, where all the definitions are in scope, is what the entire `let` expression becomes.
Actually, the definitions are in scope even as you write more definitions. Here's a somewhat contrived example.

```elm
radToDeg rad =
    let piInDegrees = 180
        conversionFactor = piInDegrees/pi
    in conversionFactor * rad
```

Be aware that if you define the same name multiple times, the innermost definition is used. Usually you should just
avoid the issue entirely by using unique names.

```elm
foo = 0

silly foo =
  let foo = 12
  in foo

silly 5 == 12 -- True
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

### Scope in Elm

In Elm as in JavaScript, **scope** refers to what is defined, and where. It is the answer to the question, "where did
this thing come from?", which you will ask constantly when reading examples or someone else's code. Scope also can tell
you why something *isn't* available, what code defines similar values, and where you can find documentation.

Scope in Elm is often simpler than in other languages because ordinary values do not change over time. Not counting
syntax (e.g. `if`, `->`, and brackets), pretty much everything in Elm is either a literal, something you imported, or
something you defined.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))