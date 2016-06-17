**Elm 0.??**, **copied, please ignore** 

## Maybe


### BANNING NULLS

type Maybe a = Nothing | Just a

type alias User =
  { name : String
  , age : Maybe Int
  }

canBuyAlcohol : User -> Bool
canBuyAlcohol user =
  case user.age of
    Nothing ->
      False

    Just age ->
      age >= 18

(source: ???)

### Where did `null` and `undefined` go?

In our applications it is often useful to be able to represent the possible
absence of a value. In JavaScript we have `undefined` to represent the result
of trying to access something that was never defined to begin with, and we have
`null` to assign to values that _might_ have a value but could also be empty. These two are often conflated and used interchangeably both at the core language level and in libraries, but both are used to communicate the lack of a value where it is possible to find one.

For example, consider the function `Array.prototype.find` which is defined in the ECMAscript 2015 spec:

```javascript
const array = [1, 2, 3, 4, 5];
const foundOne = array.find((x) => x === 1); // foundOne === 1
const foundSix = array.find((x) => x === 6); // foundSix === undefined
```

The return value of the `find` method in this scenario is either a `number`, or `undefined` to indicate to us that the item we are looking for is not present in the array. This type of API, and the presence of values like `null` and `undefined` in our programming language put the idea of emptiness on an equivalent level to the concept of the value of a variable. In dynamic languages like JavaScript this is a trivial outcome of the dynamic type system. In typed languages like C# the concept of emptiness requires additional type information to denote, but the same rules apply: a variable of a type, provided it is
allowed to be `null`, can be `null` at any time.

This requires us as programmers to perform checks when we expect a value
might be `null` or `undefined`.  Humans are not perfect, therefore a major
category of bugs manifests itself such as the Null Reference Error when we
forget to check or incorrectly expect that a value will never be null.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Where%20Did%20Null%20And%20Undefined%20Go.md]])

### `null` is not allowed in Elm

In Elm, values have types and those types are absolutely static. If a function expects an `Int` argument, the program will only compile if that function is only called with `Int` values. This prevents us from calling that function with `String` values, etc., but it also precludes the situation explained earlier where the value might be `null` or `undefined`. Not only are `null` and `undefined` not included as a part of Elm, they wouldn't work regardless because `undefined` and `null` are not of type `Int` or any other type.

### `Maybe` arises from these properties of Elm

Even though we no longer have a concept of `null` and `undefined`, we still need to be able represent optional values. Consider again the scenario where we want to attempt to find an element in an Elm `List Int`, as opposed to the JavaScript array. We still may not find the thing we were looking for, and since Elm types are static we need a single type to represent the possible absence of the `Int` we are trying to find. `Maybe` is that type. Furthermore, because types are static, a function which returns an `Int` rather than a `Maybe Int` will _always_ return an `Int`, so there is never uncertainty or need for unnecessary `null` checks. The `Maybe` type fully describes the presence of an optional
value.

### Maybe
A `Maybe` can help you with optional arguments, error handling, and records with optional fields. Think of it as a kind of`null`
```elm
-- Maybe resides in a module
import Maybe exposing ( Maybe(..) )

-- Takes an argument that can be filled with any value
type Maybe a = Just a | Nothing
```

[Type annotation](#type-annotation) explicitly tells that it will give back an `Int` or it won't.
```elm
getId : Int -> Maybe Int
getId id =
  if id >= 0 then
    Just id
  else
    Nothing
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


### Maybe

It is best to just start with the definition of Maybe. It is a union type just like in all the examples here. It is defined like this:

```elm
> type Maybe a = Nothing | Just a

> Nothing
Nothing : Maybe a

> Just
<function> : a -> Maybe a

> Just "hello"
Just "hello" : Maybe String

> Just 1.618
Just 1.618 : Maybe Float
```

If you want to have a Maybe value, you have to use the Nothing or Just constructors to create it. This means that to deal with the data, you have to use a case expression. This means the compiler can ensure that you have definitely covered both possibilities!

There are two major cases where you will see Maybe values.

(source: [an introduction to elm](https://evancz.gitbooks.io/an-introduction-to-elm/content/error_handling/maybe.html))


### Optional Fields

Say we are running a social networking website. Connecting people, friendship, etc. You know the spiel. The Onion outlined our real goals best: mine as much data as possible for the CIA. And if we want all the data, we need to ease people into it. Let them add it later. Add features that encourage them to share more and more information over time.

So let's start with a simple model of a user. They must have a name, but we are going to make the age optional.

```elm
type alias User =
  { name : String
  , age : Maybe Int
  }
```

Now say Sue logs in and decides not to provide her birthday:

```elm
sue : User
sue =
  { name = "Sue", age = Nothing }
```

Now her friends cannot wish her a happy birthday. Sad! Later Tom creates a profile and does give his age:

```elm
tom : User
tom =
  { name = "Tom", age = Just 24 }
```

Great, that will be nice on his birthday. But more importantly, Tom is part of a valuable demographic! The advertisers will be pleased.

Alright, so now that we have some users, how can we market alcohol to them without breaking any laws? People would probably be mad if we market to people under 21, so let's check for that:

```elm
canBuyAlcohol : User -> Bool
canBuyAlcohol user =
  case user.age of
    Nothing ->
      False

    Just age ->
      age >= 21
```

Now the cool thing is that we are forced to use a case to pattern match on the users age. It is actually impossible to write code where you forget that users may not have an age. Elm can make sure of it. Now we can advertise alcohol confident that we are not influencing minors directly! Only their older peers.

(source: [an introduction to elm](https://evancz.gitbooks.io/an-introduction-to-elm/content/error_handling/maybe.html))

### Partial Functions

Sometimes you want a function that gives an answer sometimes, but just does not in other cases.

Let's say Mountain Dew wants to do some ad buys for people ages 13 to 18. Honestly, it is better to start kids on Mountain Dew younger than that, but it is illegal for kids under 13 to be on our site.

So let's say we want to write a function that will tell us a user's age, but only if they are between 13 and 18:

```elm
getTeenAge : User -> Maybe Int
getTeenAge user =
  case user.age of
    Nothing ->
      Nothing

    Just age ->
      if 13 <= age && age <= 18 then
        Just age

      else
        Nothing
```

Again, we are reminded that users may not have an age, but if they do, we only want to return it if it is between 13 and 18. Now Elm can guarantee that anyone who calls getTeenAge will have to handle the possibility that the age is out of range.

This gets pretty cool when you start combining it with library functions like List.filterMap that help you process more data. For example, maybe we want to figure out the distribution of ages between 13 and 18. We could do it like this:

```elm
> alice = User "Alice" (Just 14)
... : User

> bob = User "Bob" (Just 16)
... : User

> users = [ sue, tom, alice, bob ]
... : List User

> List.filterMap getTeenAge users
[14,16] : List Int
```

We end up with only the ages we care about. Now we can feed our List Int into a function that figures out the distributions of each number.

(source: [an introduction to elm](https://evancz.gitbooks.io/an-introduction-to-elm/content/error_handling/maybe.html))

### `Maybe` in Elm

In the language nomenclature, `Maybe` is a type constructor, meaning its full signature is `Maybe a`. Here, `a` stands in for any other type and indicates to us that `Maybe` is really a container for other types that serves to add some additional meaning to a value: whether or not the value we want is present. Additionally, `Maybe` is a union type, and in fact its full definition is as simple as:

```elm
type Maybe a = Just a | Nothing
```

By defining this, we are establishing a type, `Maybe a`, and two possible _type constructors_ for that type, `Just a` and `Nothing`. Invoking either type constructor like a function or value will give us a `Maybe` value which is the particular member that we used to construct. If we want to represent a non-empty value of 5, we can invoke `Just 5` to get a `Maybe Int`. If we don't have a value, we simply pass around `Nothing` since it has no arguments.

As a union type, `Maybe` is also a data structure in the same way as `List`. `Maybe` is a container for a single element, and as a container we are able to define a couple functions that are available for container structures like `List` and `Task`.

#### `map`
```elm
map : (a -> b) -> Maybe a -> Maybe b`
```

Given a `Maybe a` value, transform its contained item from `a` to `b` in
the case that it is `Just a`, and pass through if it is `Nothing`.

#### `andThen`

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

#### `withDefault`

```elm
withDefault : a -> Maybe a -> a
```

Given a default value of type `a` and some `Maybe a`, return either the value
contained by the `Maybe` when it is `Just a` or the default value when it is
`Nothing`.

## Illustrating `Maybe` by example

Let's return once more to finding an item in a list. To build a basic function
to find an element in a `List Int`, we can start with
the following type signature:

```elm
find : Int -> List Int -> Maybe Int
```

We should read the type signature as "`find` is a function which accepts an
`Int` and a `List` of `Int`s and returns a `Maybe Int`". In the JavaScript
example, `find` returns either a number or `undefined` if the value isn't
in the array. In Elm, we must always return the same type from a function
regardless of the outcome of the logic. We can use the two members of `Maybe`,
`Just` and `Nothing`, to handle the two possibilities and still maintain type
consistency.

```elm
find : Int -> List Int -> Maybe Int
find valueToFind listToSearch =
  case listToSearch of
    currentItem :: remainingItems ->
      if currentItem == valueToFind then
        Just currentItem
      else
        find valueToFind remainingItems

    [] ->
      Nothing
```

We've defined `find` recursively, and used pattern matching to search the list. The first case, `currentItem :: remainingItems`, is matched when there is at least one value in the list, and makes available the item at the front of the list, `currentItem`, and another list `remainingItems` which is everything else in that list. If there was one item left, then `remainingItems` will be an empty list. Now that we have access to the front item, we can check if it's equal to the one we're trying to find. If it is, we can return `Just currentItem`, which is of type `Maybe Int` and represents the case where we do have the item of interest. If it isn't equal, we continue our search over the remaining items
recursively.

The base case of our recursion is that when there is nothing left in the list. We've exhausted every possibility and found nothing that matches. In this case we return the aptly named `Nothing`, which is also of type `Maybe Int` in this context, but has no data associated with it because we've got no information to return.

So far there's not a lot of difference between our function's return value and one in JavaScript that returns a number or `undefined`. The semantics are roughly the same, and the main difference has been the implementation details of using a single type in Elm vs. multiple types in JavaScript to represent value vs. lack of value. The real difference for Elm programs comes in the way that `Maybe` is handled.

## Dealing with a `Maybe` value

In JavaScript the language imposes no expectation on how we invoke and use
values, whether they might be `undefined`, `null`, or some real value because
the language is dynamic. From the first example, we could continue on to use
`foundSix` as a number even though it is not, and introduce bugs into our
program.

```javascript
const shouldBeSeven = foundSix + 1; // shouldBeSeven is NaN
```

This case is easy to miss because it is the programmer's responsibility to
understand that the value might not be a number. Recall, though, that in Elm `Maybe` is defined to be a union type. This means that in order to work with it we must pattern match, and in doing so we must consciously acknowledge every possible pattern that we can encounter. Further more, we can't attempt to fraudulently add a `Maybe Int` to another `Int` because the compiler doesn't know how to add those two types together. The operation doesn't make logical sense, and instead of reflecting that fact at run-time through a construct like `NaN`, the compiler simply refuses to compile the code.

To attempt to add 1 to the result of our search for 6, we must first check that we found 6 at all and in doing so handle both the case where we were successful and the case where we were not.

```elm
foundSix : Maybe Int
foundSix =
  find 6 [1, 2, 3, 4, 5]

shouldBeSeven : Maybe Int
shouldBeSeven =
  case foundSix of
    Just value ->
      value + 1
    Nothing ->
      Nothing
```

In the above example we need to match on `Just` in order to extract the value if
it is available. This leaves only one other possible pattern, `Nothing`, and we
just pass through another `Nothing`. Elm has raised the concept of emptiness
from something like `null` that could occur in any variable and brought it
outside the actual data type of the value we want to use. It adds that
information on top of the underlying value and forces us to deal with both cases
via pattern matching.

Note also that in the above example we simply passed through for the `Nothing`
case. In this case we are passing `Nothing` straight through and only interested
in transforming the value under `Just`. This case is handled for us more
succinctly by `Maybe.map`. We could rewrite the code as

```elm
shouldBeSeven : Maybe Int
shouldBeSeven =
  Maybe.map (\value -> value + 1) foundSix
```

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Where%20Did%20Null%20And%20Undefined%20Go.md]])

## What about representing failed operations?

In JavaScript `null` and `undefined` are sometimes used to encode the fact that
an operation has or has not successfully completed. Consider the canonical
Node.js callback style:

```javascript
function callback(err, data) {
  if (err) {
    // ...handle error case
  } else {
    // ... error is null or undefined so we know everything is Ok
  }
}
```

The absence of an error indicates that there was no error. In Elm we could write
a similar function that might look like this:

```elm
callback : Maybe MyError -> Maybe MyData -> MyOutput
callback maybeError maybeData =
  case maybeError of
    Just error ->
      -- ...
    Nothing ->
      case maybeData of
        Just data ->
          -- ...
        Nothing ->
          -- ...
```

This kind of handling is extremely awkward in Elm, and writing this way discards
what we've already learned about using `Maybe` to represent emptiness.
Furthermore, there is no reason our function must accept exactly one `Just
MyError` or `Just MyData` and one `Nothing`, making this pattern unpredictable.
To deal with cases like this Elm's core libraries also provide the `Result` type
to allow us to represent the possibility of a failure to return something the
same way would use `Maybe` to represent having nothing to return. The definition
of `Result` is also familiar:

```elm
type Result a b = Err a | Ok b
```

We once again have two type constructors to represent two possibilities. On one
side we have the `Err` case and any data type we'd like to associate with error,
and on the other we have `Ok` and a data type that we are interested in
computing. We can then pattern match once to handle both cases and have access
to the information in either case. Like `Maybe`, we also know that if a function
doesn't return a `Result` then there is no possibility its computation will fail
and we can be confident in calling it without safeguards and error handling.
Let's reimagine the previous node-style callback example using `Result`:

```elm
callback : Result MyError MyData -> MyOutput
callback result =
  case result of
    Err error ->
      -- error is a value of type MyError
    Ok data ->
      -- data is a value of type MyData
```

`Result` is most commonly found in the core libraries when working with
`Json.Decode` functions, as parsing JSON is something that may fail at runtime
if the input is malformed. We can't detect that the input is malformed at
compile time, and Elm does not provide a concept of exceptions and `try/catch`
so we can use `Result` to model the output.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Where%20Did%20Null%20And%20Undefined%20Go.md]])

## Conclusions

Languages like JavaScript handle the concept of emptiness with low-level values
like `null` and `undefined`, and may also use those to represent failure and
success. These concepts are hard to represent in a statically typed language
without introducing a common class of bugs. Elm uses container types to bring
the concepts of emptiness and failure outside of values and add that information to a type instead of interleaving it. Instead of having a value which might be a number or `null`, we have `Just` a number or `Nothing`, and the compiler helps us deal with that. Instead of representing failure and success by emptiness or resorting to an exception system, we have either an `Err` with some information,
or some `Ok` data.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/Where%20Did%20Null%20And%20Undefined%20Go.md]])

## Why Maybe type

Maybe let you differentiate from an empty list or an empty string and an undefined one.

    type alias Model = { things : Maybe (List Thing) }

Now we can represent, "we haven't loaded things yet," naturally. On startup the list of things is Nothing. When the load completes it becomes Just [thing1, thing2, ...].

If the server replies that there genuinely were no things, then that's Just []. Now the empty list actually means the empty list, it's no longer doing double-duty as the "not loaded" case.

You can do this in JavaScript of course, but you won't. Long experience will have taught you that setting a property to null may be correct, but it's just asking for runtime exceptions. Hence the separate loading flag, which gets forgotten.

In Elm, you can not only handle the nothing case safely, but the compiler will actively nag you to remember to write the "Loading" message:

    module View exposing (..)

    ...

    someView model =
      case model.things of
        Just things -> ...

…and when we compile:

    -- MISSING PATTERNS ---------------------------------------------- src/View.elm

    This `case` does not have branches for all possibilities.

    ...

You need to account for the following values:

    Maybe.Nothing

Add a branch to cover this pattern!
We get an error message about missing out the loading case, exactly where the solution needs to go.

On the surface it seems like we've made a simple data-model change, but by reshaping the data it becomes much easier to ensure an improved user experience. Win.

(source: [[http://blog.jenkster.com/2016/06/how-elm-slays-a-ui-antipattern.html]])


## Result

A Result is useful when you have logic that may "fail". For example, parsing a String into an Int may fail. What if the string is filled with the letter B? In cases like this, we want a function with this type:

```elm
String.toInt : String -> Result String Int
```

This means that `String.toInt` will take in a string value and start processing the string. If it cannot be turned into an integer, we provide a String error message. If it can be turned into an integer, we return that Int. So the Result String Int type is saying, "my errors are strings and my successes are integers."

To make this as concrete as possible, let's see the actual definition of Result. It is actually pretty similar to the Maybe type, but it has two type variables:

```elm
type Result error value
  = Err error
  | Ok value
```

You have two constructors: Err to tag errors and Ok to tag successes. Let's see what happens when we actually use String.toInt in the REPL:

```elm
> import String

> String.toInt "128"
Ok 128 : Result String Int

> String.toInt "64"
Ok 64 : Result String Int

> String.toInt "BBBB"
```

Err "could not convert string 'BBBB' to an Int" : Result String Int
So instead of throwing an exception like in most languages, we return data that indicates whether things have gone well or not. Let's imagine someone is typing their age into a text field and we want to show a validation message:

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

Again, we have to use case so we are guaranteed to handle the special case where the number is bad.

(source: [an introduction to elm](https://evancz.gitbooks.io/an-introduction-to-elm/content/error_handling/result.html))


# Where did `null` and `undefined` go?

In our applications it is often useful to be able to represent the possible
absence of a value. In JavaScript we have `undefined` to represent the result
of trying to access something that was never defined to begin with, and we have
`null` to assign to values that _might_ have a value but could also be empty.
These two are often conflated and used interchangeably both at the core language
level and in libraries, but both are used to communicate the lack of a value
where it is possible to find one.

For example, consider the function `Array.prototype.find` which is defined in
the ECMAscript 2015 spec:

```javascript
const array = [1, 2, 3, 4, 5];
const foundOne = array.find((x) => x === 1); // foundOne === 1
const foundSix = array.find((x) => x === 6); // foundSix === undefined
```

The return value of the `find` method in this scenario is either a `number`, or
`undefined` to indicate to us that the item we are looking for is not present
in the array. This type of API, and the presence of values like `null` and
`undefined` in our programming language put the idea of emptiness on an
equivalent level to the concept of the value of a variable. In dynamic languages
like JavaScript this is a trivial outcome of the dynamic type system. In typed
languages like C# the concept of emptiness requires additional type information
to denote, but the same rules apply: a variable of a type, provided it is
allowed to be `null`, can be `null` at any time.

This requires us as programmers to perform checks when we expect a value
might be `null` or `undefined`.  Humans are not perfect, therefore a major
category of bugs manifests itself such as the Null Reference Error when we
forget to check or incorrectly expect that a value will never be null.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## `null` is not allowed in Elm

In Elm, values have types and those types are absolutely static. If a function
expects an `Int` argument, the program will only compile if that function is
only called with `Int` values. This prevents us from calling that function with
`String` values, etc., but it also precludes the situation explained earlier
where the value might be `null` or `undefined`. Not only are `null` and
`undefined` not included as a part of Elm, they wouldn't work regardless because
`undefined` and `null` are not of type `Int` or any other type.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## `Maybe` arises from these properties of Elm

Even though we no longer have a concept of `null` and `undefined`, we still need
to be able represent optional values. Consider again the scenario where we want
to attempt to find an element in an Elm `List Int`, as opposed to the JavaScript
array. We still may not find the thing we were looking for, and since Elm types
are static we need a single type to represent the possible absence of the `Int`
we are trying to find. `Maybe` is that type. Furthermore, because types are
static, a function which returns an `Int` rather than a `Maybe Int` will
_always_ return an `Int`, so there is never uncertainty or need for unnecessary
`null` checks. The `Maybe` type fully describes the presence of an optional
value.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## `Maybe` in Elm

In the language nomenclature, `Maybe` is a type constructor, meaning its full
signature is `Maybe a`. Here, `a` stands in for any other type and indicates to
us that `Maybe` is really a container for other types that serves to add some
additional meaning to a value: whether or not the value we want is present.
Additionally, `Maybe` is a union type, and in fact its full definition is as
simple as:

```elm
type Maybe a = Just a | Nothing
```

By defining this, we are establishing a type, `Maybe a`, and two possible _type
constructors_ for that type, `Just a` and `Nothing`. Invoking either type
constructor like a function or value will give us a `Maybe` value which is the
particular member that we used to construct. If we want to represent a non-empty
value of 5, we can invoke `Just 5` to get a `Maybe Int`. If we don't have a
value, we simply pass around `Nothing` since it has no arguments.

As a union type, `Maybe` is also a data structure in the same way as `List`.
`Maybe` is a container for a single element, and as a container we are able to
define a couple functions that are available for container structures like
`List` and `Task`.

#### `map`
```elm
map : (a -> b) -> Maybe a -> Maybe b`
```

Given a `Maybe a` value, transform its contained item from `a` to `b` in
the case that it is `Just a`, and pass through if it is `Nothing`.

#### `andThen`

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

#### `withDefault`

```elm
withDefault : a -> Maybe a -> a
```

Given a default value of type `a` and some `Maybe a`, return either the value
contained by the `Maybe` when it is `Just a` or the default value when it is
`Nothing`.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Illustrating `Maybe` by example

Let's return once more to finding an item in a list. To build a basic function
to find an element in a `List Int`, we can start with
the following type signature:

```elm
find : Int -> List Int -> Maybe Int
```

We should read the type signature as "`find` is a function which accepts an
`Int` and a `List` of `Int`s and returns a `Maybe Int`". In the JavaScript
example, `find` returns either a number or `undefined` if the value isn't
in the array. In Elm, we must always return the same type from a function
regardless of the outcome of the logic. We can use the two members of `Maybe`,
`Just` and `Nothing`, to handle the two possibilities and still maintain type
consistency.

```elm
find : Int -> List Int -> Maybe Int
find valueToFind listToSearch =
  case listToSearch of
    currentItem :: remainingItems ->
      if currentItem == valueToFind then
        Just currentItem
      else
        find valueToFind remainingItems

    [] ->
      Nothing
```

We've defined `find` recursively, and used pattern matching to search the list.
The first case, `currentItem :: remainingItems`, is matched when there is at
least one value in the list, and makes available the item at the front of the
list, `currentItem`, and another list `remainingItems` which is everything else
in that list. If there was one item left, then `remainingItems` will be an empty
list. Now that we have access to the front item, we can check if it's equal to
the one we're trying to find. If it is, we can return `Just currentItem`, which
is of type `Maybe Int` and represents the case where we do have the item of
interest. If it isn't equal, we continue our search over the remaining items
recursively.

The base case of our recursion is that when there is nothing left in the list.
We've exhausted every possibility and found nothing that matches. In this case
we return the aptly named `Nothing`, which is also of type `Maybe Int` in this
context, but has no data associated with it because we've got no information to
return.

So far there's not a lot of difference between our function's return value and
one in JavaScript that returns a number or `undefined`. The semantics are
roughly the same, and the main difference has been the implementation details of
using a single type in Elm vs. multiple types in JavaScript to represent value
vs. lack of value. The real difference for Elm programs comes in the way that
`Maybe` is handled.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Dealing with a `Maybe` value

In JavaScript the language imposes no expectation on how we invoke and use
values, whether they might be `undefined`, `null`, or some real value because
the language is dynamic. From the first example, we could continue on to use
`foundSix` as a number even though it is not, and introduce bugs into our
program.

```javascript
const shouldBeSeven = foundSix + 1; // shouldBeSeven is NaN
```

This case is easy to miss because it is the programmer's responsibility to
understand that the value might not be a number. Recall, though, that in Elm
`Maybe` is defined to be a union type. This means that in order to work with it
we must pattern match, and in doing so we must consciously acknowledge every
possible pattern that we can encounter. Further more, we can't attempt to
fraudulently add a `Maybe Int` to another `Int` because the compiler doesn't
know how to add those two types together. The operation doesn't make logical
sense, and instead of reflecting that fact at run-time through a construct like
`NaN`, the compiler simply refuses to compile the code.

To attempt to add 1 to the result of our search for 6, we must first check that
we found 6 at all and in doing so handle both the case where we were successful
and the case where we were not.

```elm
foundSix : Maybe Int
foundSix =
  find 6 [1, 2, 3, 4, 5]

shouldBeSeven : Maybe Int
shouldBeSeven =
  case foundSix of
    Just value ->
      value + 1
    Nothing ->
      Nothing
```

In the above example we need to match on `Just` in order to extract the value if
it is available. This leaves only one other possible pattern, `Nothing`, and we
just pass through another `Nothing`. Elm has raised the concept of emptiness
from something like `null` that could occur in any variable and brought it
outside the actual data type of the value we want to use. It adds that
information on top of the underlying value and forces us to deal with both cases
via pattern matching.

Note also that in the above example we simply passed through for the `Nothing`
case. In this case we are passing `Nothing` straight through and only interested
in transforming the value under `Just`. This case is handled for us more
succinctly by `Maybe.map`. We could rewrite the code as

```elm
shouldBeSeven : Maybe Int
shouldBeSeven =
  Maybe.map (\value -> value + 1) foundSix
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## What about representing failed operations?

In JavaScript `null` and `undefined` are sometimes used to encode the fact that
an operation has or has not successfully completed. Consider the canonical
Node.js callback style:

```javascript
function callback(err, data) {
  if (err) {
    // ...handle error case
  } else {
    // ... error is null or undefined so we know everything is Ok
  }
}
```

The absence of an error indicates that there was no error. In Elm we could write
a similar function that might look like this:

```elm
callback : Maybe MyError -> Maybe MyData -> MyOutput
callback maybeError maybeData =
  case maybeError of
    Just error ->
      -- ...
    Nothing ->
      case maybeData of
        Just data ->
          -- ...
        Nothing ->
          -- ...
```

This kind of handling is extremely awkward in Elm, and writing this way discards
what we've already learned about using `Maybe` to represent emptiness.
Furthermore, there is no reason our function must accept exactly one `Just
MyError` or `Just MyData` and one `Nothing`, making this pattern unpredictable.
To deal with cases like this Elm's core libraries also provide the `Result` type
to allow us to represent the possibility of a failure to return something the
same way would use `Maybe` to represent having nothing to return. The definition
of `Result` is also familiar:

```elm
type Result a b = Err a | Ok b
```

We once again have two type constructors to represent two possibilities. On one
side we have the `Err` case and any data type we'd like to associate with error,
and on the other we have `Ok` and a data type that we are interested in
computing. We can then pattern match once to handle both cases and have access
to the information in either case. Like `Maybe`, we also know that if a function
doesn't return a `Result` then there is no possibility its computation will fail
and we can be confident in calling it without safeguards and error handling.
Let's reimagine the previous node-style callback example using `Result`:

```elm
callback : Result MyError MyData -> MyOutput
callback result =
  case result of
    Err error ->
      -- error is a value of type MyError
    Ok data ->
      -- data is a value of type MyData
```

`Result` is most commonly found in the core libraries when working with
`Json.Decode` functions, as parsing JSON is something that may fail at runtime
if the input is malformed. We can't detect that the input is malformed at
compile time, and Elm does not provide a concept of exceptions and `try/catch`
so we can use `Result` to model the output.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Conclusions

Languages like JavaScript handle the concept of emptiness with low-level values
like `null` and `undefined`, and may also use those to represent failure and
success. These concepts are hard to represent in a statically typed language
without introducing a common class of bugs. Elm uses container types to bring
the concepts of emptiness and failure outside of values and add that information
to a type instead of interleaving it. Instead of having a value which might be a
number or `null`, we have `Just` a number or `Nothing`, and the compiler helps
us deal with that. Instead of representing failure and success by emptiness or
resorting to an exception system, we have either an `Err` with some information,
or some `Ok` data.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

What do you think is the type of the List `length` function? Because `length` takes a list of any type and returns either `Just` the first element or `Nothing`, so what could it be? Let's check!

