**Elm 0.17**, **shareable** 

## Maybe and Result

The concept of emptiness (`null` or `undefined`) and of failure are hard to represent in a statically typed language. Elm uses container types Maybe and Result for that purpose. 

To represent emptiness, we have the Maybe type with values tagged either as `Just` a value or `Nothing`. 

To represent failure and success, we have the Result type with values tagged either an `Err` with some information, or some `Ok` data.

### Maybe for optional values

In Elm, value types are absolutely static. If a value is expected to be of type `String`, then the compiler will complain whenever a value of a different type is assigned. A difficulty, then, is to find a way to express a value that has not yet been defined. Using an empty string to denote a value not yet defined is bound to bite us sooner or later. 

We need to be able represent optional values and since Elm types are static we need a single type to represent the possible absence of the `Int` we are trying to find. `Maybe` is that type. It fully describes the presence of an optional value.

In the language nomenclature, `Maybe` is a type constructor, meaning its full signature is `Maybe a`. Here, `a` stands in for any other type and indicates to us that `Maybe` is really a container for other types that serves to add some additional meaning to a value: whether or not the value we want is present. Additionally, `Maybe` is a union type.

```elm
$ elm repl
-- Takes an argument that can be filled with any value
> type Maybe a = Just a | Nothing
> Just "hello"
Just "hello" : Maybe.Maybe String
> Nothing
Nothing : Maybe.Maybe a
```

Think of `Nothing` as a way to indicate when there's no correct value to return, and `Just` as a way to show that we could return a value.

#### elm functions

`List.head` or `List.tail` return `Nothing` for an empty list and `Just a` for a non empty one. 

```elm
$ elm repl
> List.head
<function> : List a -> Maybe.Maybe a
> List.head [5,4,3,2,1]
Just 5 : Maybe.Maybe number
> List.tail [5,4,3,2,1]
Just [4,3,2,1] : Maybe.Maybe (List number)
> List.head []
Nothing : Maybe.Maybe a
> List.tail []
Nothing : Maybe.Maybe (List a)
```
#### making values optional

You have to use a case expression on any optional value to ensure that you have covered all scenarios. It is impossible to compile code where you forget to deal with cases where the value can end up being undefined.

```elm
type alias User = { name : String , age : Maybe In }

canBuyAlcohol : User -> Bool
canBuyAlcohol user =
  case user.age of
    Nothing ->
      False

    Just age ->
      age >= 18
      
sue : User
sue = { name = "Sue", age = Nothing }

tom : User
tom = { name = "Tom", age = Just 24 }

canBuyAlcohol sue
-- False
canBuyAlcohol tom
-- True

List.filterMap canBuyAlcohol [sue,tom]
[tom] : List Int
```

#### differentiating empty from undefined

Maybe let you differentiate from an empty list or an empty string and an undefined one.

    type alias Model = { things : Maybe (List Thing) }

Now we can represent, "we haven't loaded things yet," naturally. On startup the list of things is Nothing. When the load completes it becomes Just [thing1, thing2, ...].

If the server replies that there genuinely were no things, then that's Just []. Now the empty list actually means the empty list, it's no longer doing double-duty as the "not loaded" case.

(source: [how-elm-slays-a-ui-antipattern](http://blog.jenkster.com/2016/06/how-elm-slays-a-ui-antipattern.html)))

#### returning Maybe values

If you want to have a Maybe value, you have to use the Nothing or Just constructors to create it. 

```elm
getId : Int -> Maybe Int
getId id =
  if id >= 0 then
    Just id
  else
    Nothing
```
(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


#### Operations on Maybe


##### `map`
```elm
map : (a -> b) -> Maybe a -> Maybe b`
```

Given a `Maybe a` value, transform its contained item from `a` to `b` in
the case that it is `Just a`, and pass through if it is `Nothing`.

##### `andThen`

```elm
andThen : Maybe a -> (a -> Maybe b) -> Maybe b
```

Given a `Maybe a`, chain a computation which produces a new `Maybe b` from the
contained element when the value is `Just a`, and pass through `Nothing` when it
is `Nothing`. This allows us to chain multiple `Maybe`-production computations
together without worrying about `null` checks along the way. If any of the
`Maybe` values in the chain are `Nothing` we will get `Nothing` back without
error.

`Maybe` also falls into a group of types where some possibilities are
represented and one of those possibilities is most often of primary interest. In
the case of `Maybe`, it's most common to be interested in working with the value
under the `Just` case. Types with this property often come with a `withDefault`
function for collapsing the `Maybe` using some acceptable default value.

##### `withDefault`

```elm
withDefault : a -> Maybe a -> a
```

Given a default value of type `a` and some `Maybe a`, return either the value
contained by the `Maybe` when it is `Just a` or the default value when it is
`Nothing`.


### Result for failure or success

Elm's core libraries provide the `Result` type to encode the result of an operation that might "fail". If the function fails, it returns an error.  If it succeeds, it returns a result. 

```elm
type Result a b = Err a | Ok b
```

On one side we have the `Err` case and any data type we'd like to associate with error,
and on the other we have `Ok` and a data type that we are interested in computing.



#### example 

`Result` is commonly found in functions that operate some parsing, as parsing is something that may fail if the input is malformed. We can use `Result` to model the output. Take the built-in function `String.toInt`.

```elm
> import String
> String.toInt
<function:toInt> : String -> Result.Result String Int
> String.toInt "128"
Ok 128 : Result String Int
> String.toInt "64"
Ok 64 : Result String Int
> String.toInt "BBBB"
Err "could not convert string 'BBBB' to an Int" : Result String Int
```

#### covering all cases

We have to pattern match once to handle both successful and failed outcome. 

```elm
view : String -> Html msg
view userInputAge =
  case String.toInt userInputAge of
    Err msg ->
      span [class "error"] [text msg]

    Ok age ->
      if age < 0 then
        span [class "error"] [text "I bet you are older than that!"]

      else if age > 140 then
        span [class "error"] [text "Seems unlikely..."]

      else
        text "OK!"
```

Or

```elm
callback : Result MyError MyData -> MyOutput
callback result =
  case result of
    Err error ->
      -- error is a value of type MyError
    Ok data ->
      -- data is a value of type MyData
```

### Further Readings

* [an introduction to elm](https://evancz.gitbooks.io/an-introduction-to-elm/content/error_handling/maybe.html)
* [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md) and [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/Where%20Did%20Null%20And%20Undefined%20Go.md) (identical)