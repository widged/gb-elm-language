**Elm 0.??**, **copied, please ignore** 




## Let it be

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
toPrint = [let square x = x * x in (square 5, square 3, square 2)]
[(25,9,4)]
```

Like we said before, you can pattern match with *let* bindings. They're very useful for quickly dismantling a tuple into components and binding them to names and such.

```elm
toPrint = (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
```

(source: [learnyouanelm-04](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/04-syntax-in-functions.md))


# Eager by default


Elm syntax is very similar to Haskell. There is one important difference to keep in mind. Elm is *eager*, unlike Haskell, which is lazy. That means, in Elm, if you call a function, the arguments are fully evaluated before they are passed to the function. Most programming languages are like this, so you won't likely need to think about this very much.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/01-introduction.md))

# Pattern matching

Pattern matching on record fields
```elm
-- Requires the argument with x and y fields
multiply {x,y} =
    x * y
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

# Links

* [elm-lang style guide](http://elm-lang.org/docs/style-guide)
* [elm style guide @NodeRedInk](https://github.com/NoRedInk/elm-style-guide)
*
