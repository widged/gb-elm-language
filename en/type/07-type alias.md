**Elm 0.??**, **copied, please ignore** 

## Type Aliases

With type aliases, we can give a convenient alternative name for another existing type (primitive or complex shape). The general shape is `type alias CustomType = ExistingType`. 

It helps you model the problem and reason about how data flows through your program.


```elm
type alias Name = String
type alias IncrementCount = Int
type alias Point2D = { x : Float, y : Float }
origin : Point2D
origin = { x = 0, y = 0 }
```

See [Records](05-record.md)


### Further Readings

* [understanding-the-elm-type-system.html](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html)
