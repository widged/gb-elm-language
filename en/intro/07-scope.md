**Elm 0.17**, **shareable** 

# Scope

## Variable definitions

**Scope** refers to what is defined, and where. 

### Top-Level Definitions

```elm
answer = 42
```

Elm is immutable, the variable cannot be set to another value in another part of the file. 42 is a literal. 

### Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. 

### Let ... in statement

Let bindings let you bind values to variables. The form is let \<bindings\> in \<expression\>. 

They can be fixed values or functions. 

```elm
radToDeg rad =
    let piInDegrees = 180
        conversionFactor = piInDegrees/pi
    in conversionFactor * rad
```

They are particularly useful for naming the results of more complicated expressions. For instance, this is how we could define a function that gives us a cylinder's surface area based on its height and radius:

```elm
cylinder : Float -> Float -> Float
cylinder r h =
    let
        sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  
        sideArea + 2 * topArea
```

Or 

```elm
distanceFrom (originX, originY) (x, y) =
    let dx = x - originX
        dy = y - originY
    in sqrt (dx^2 + dy^2)
```

Or

```elm
let
  activeUsers = List.filter (\u -> u.state /= 1) model.users
in
  { model | user = activeUsers}
```

After the `let`, you can place as many definitions as you like. The names that you define in the *let* part are accessible to both the let and in part. Notice that the names are also aligned in a single column.

Let bindings are expressions themselves. That means you can cram it in almost anywhere.

```elm
toPrint = 4 * (let a = 9 in a + 1) + 2
42
```

You can even write type annotations. However, you can't define new types.

# Links

* [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md) 
* [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md)


