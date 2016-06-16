**Elm 0.??**, **copied, please ignore** 







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
