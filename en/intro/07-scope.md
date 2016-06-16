**Elm 0.??**, **copied, please ignore** 

# Scope

## Variable definitions

**Scope** refers to what is defined, and where. 

### Top-Level Definitions (literal)

```elm
answer = 42
```

Elm is immutable, the variable cannot be set to another value in another part of the file. 


### Local Definitions

The most common form of a local definition is a function argument. Exactly like JavaScript, any argument is visible from anywhere inside the function.

The other form of local definitions are created using a `let... in...` statement. 

### Let ... in statement

In this example, some values are function arguments, some are defined in the `let`, and some (the math operators) are imported automatically from Basics. (And `2` is a literal.)

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

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Scope.md) and [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))


## Let-in

Let bindings let you bind to variables anywhere and are expressions themselves, so they are very useful for naming the results of more complicated expressions. Just like any construct in Elm that is used to bind values to names, let bindings can be used for pattern matching. Let's see them in action! This is how we could define a function that gives us a cylinder's surface area based on its height and radius:

```elm
cylinder : Float -> Float -> Float
cylinder r h =
    let
        sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  
        sideArea + 2 * topArea
```

The form is let \<bindings\> in \<expression\>. The names that you define in the *let* part are accessible to the expression after the *in* part. Notice that the names are also aligned in a single column.

Let bindings are expressions themselves. Remember when we did the if statement and it was explained that an if then else statement is an expression? That means you can cram it in almost anywhere.


```elm
toPrint = [if 5 > 3 then "Woo" else "Boo", if 'a' > 'b' then "Foo" else "Bar"]
["Woo", "Bar"]
toPrint = 4 * (if 10 > 5 then 10 else 0) + 2
42
```

You can also do that with let bindings.

```elm
toPrint = 4 * (let a = 9 in a + 1) + 2
42
```

They can also be used to introduce functions in a local scope:

```elm
$ elm repl
> toPrint = let square x = x * x in [ square 5, square 3, square 2]
[25,9,4] : List number
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))

They can help simplify complex expressions
```elm
let
  activeUsers = List.filter (\u -> u.state /= 1) model.users
in
  { model | user = activeUsers}
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
