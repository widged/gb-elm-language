**Elm 0.??**, **copied, please ignore** 

# Constrained Types

Elm has three special type variables that indicate that the value needs to one of a few different types, but not just any type.

A `number` is either an `Int` or `Float`. A number literal without a decimal point is a `number`. Numbers support basic arithmetic (except division, which is handled separately for each type).

A `comparable` can be a number, character, string, or recursively a list or tuple of comparables. Surprisingly enough, comparables can be compared with operations like `(>)`. Elm's dictionaries and sets are implemented as binary search trees, so the keys or elements must be comparable.

An `appendable` can be a string, text (i.e. with typesetting information), or a list (containing any type). Two appendables of the same type can be appended with `(++)`.

To use any of these types, just use their name in an annotation instead of a specific type or type variable.

If one of these types appears multiple times in a type annotation, all occurrences must resolve to the same type. You can allow them to be different by sticking something on to the end of the type, like `appendable2` or similar. For example, if you enter `(4, 2)` into the Elm REPL, it will infer the type `(number, number')`. The apostrophe indicates that the second number need not be the same type as the first.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md]])

## Constrained types

Elm has three special type variables that indicate that the value needs to one of a few different types, but not just any type.

`number` is a type variable built-in to Elm that specifically represents either an `Int` or a `Float`, but nothing else. A number literal without a decimal point is a `number`. Numbers support basic arithmetic (except division, which is handled separately for each type).

`comparable` represents types that can be compared for equality with the `==` operator. This can be `String`, `Char`, `Int`, `Float`, `Time`, or a list or tuple containing only comparable values. Surprisingly enough, comparables can be compared with operations like `(>)`. Elm's dictionaries and sets are implemented as binary search trees, so the keys or elements must be comparable.

`appendable` represents items that can be appended to each other with the `++` operator. These are `String`, `List`, and `Text`.


To use any of these types, just use their name in an annotation instead of a specific type or type variable.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

## Constrained types

Elm has three special type variables that indicate that the value needs to one of a few different types, but not just
any type.

A `number` is either an `Int` or `Float`. A number literal without a decimal point is a `number`. Numbers support basic arithmetic (except division, which is handled separately for each type).

A `comparable` can be a number, character, string, or recursively a list or tuple of comparables. Surprisingly enough, comparables can be compared with operations like `(>)`. Elm's dictionaries and sets are implemented as binary search trees, so the keys or elements must be comparable.

An `appendable` can be a string, text (i.e. with typesetting information), or a list (containing any type). Two appendables of the same type can be appended with `(++)`.

To use any of these types, just use their name in an annotation instead of a specific type or type variable.

If one of these types appears multiple times in a type annotation, all occurrences must resolve to the same type. You can allow them to be different by sticking something on to the end of the type, like `appendable2` or similar. For example, if you enter `(4, 2)` into the Elm REPL, it will infer the type `(number, number')`. The apostrophe indicates that the second number need not be the same type as the first.

(source: ???)


## Constrained types

There are three special lowercase types: number, comparable, and appendable.
* Numbers allow you to use arithmetic on Ints and Floats.
* Comparable allows you to order numbers and strings, like a < b.
* Appendable things can be combined with a ++ b.

(source: ???)

## Constrained types
`comparable` - `ints`, `floats`, `chars`, `strings`, `lists`, `tuples`
<br/>
`appendable` - `strings`, `lists`, `text`.
<br/>
Kind of dynamic types are represented as `a`, `b`, `c` etc. meaning that you can pass any value, even functions

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))
