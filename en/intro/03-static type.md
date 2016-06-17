**Elm 0.17**, **shareable** 


## Strong / Static type system

A static type system means that the type of every expression is known at compile time, allowing the compiler to reason about the program before compiling it. It helps prevent any kind of error related to data types and provides high level of confidence that your program will work as expected.

If you write a program where you try to divide a boolean type with some number, the compiler informs that there is a problem and refuses to build the program. This is why Elm is said to have “no runtime exceptions”. All errors must be fixed before the program can run. 

```elm
$ elm repl
> 2 + "three"
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm

The right argument of (+) is causing a type mismatch.

3|   2 + "three"
         ^^^^^^^
(+) is expecting the right argument to be a:

    number

But the right argument is:

    String

Hint: To append strings in Elm, you need to use the (++) operator, not (+).
<http://package.elm-lang.org/packages/elm-lang/core/latest/Basics#++>

Hint: I always figure out the type of the left argument first and if it is
acceptable on its own, I assume it is "correct" in subsequent checks. So the
problem may actually be in how the left and right arguments interact.
```

What Elm is telling us is that `+` expects its left and right side to be numbers. The value `"three"` is not a number.

### Types

A type is a kind of label that every expression has. It tells us in which category of things that expression fits. Here is a quick overview of types available by default in any Elm program. They are described each in greater details in the [type section](../type/cover.md). 

Primitives:
* Int: `2` 
* Float: `2.3`
* Bool: `True`
* Strings: `"hello"`
* Chars: `'a'`
* Named Functions: `isNegative n = n < 0`, `avg x y = (x+y)/2`
* Anonymous functions:`(\ x y -> (x + y) / 2)`

Collections:
* Lists (all elements must have the same type): `[1, 2] or ["one","two","three"]`
* Tuples (fixed number of values, any mix of types): `("Dave",True) `
* Records (key-value pairs): `{x = 0,y = 10}`

Special types
* Aliases: `type alias Point={x=0,y=0}`
* Union type: `type Msg = Decrement | Increment`
* Unit type: `()`

Constrained types
* number: `2 + 2.3 -- 4.3 : Float`
* comparable: `3 > 2 -- True : Bool`
* appendable: `"hello" ++ " elm" -- "hello elm" : String`

### Type inference

We don't have to explicitly write out the types of our functions and expressions to get things done. Elm, like most ML dialects, has type inference. The compiler will automatically infer the type of every value in your program. If we write a number, we don't have to tell Elm it's a number. It can *infer* that on its own.

```bash
$ elm repl
> 2 + 2.3
4.3 : Float
```

Both `2` and `2.3` are identified as numbers. The answer is a `Float`. 

A number literal without a decimal point can be treated as an Int or a Float in different operations.

```elm
33 / 2 -- 16.5 with floating point division
33 // 2 -- 16 with integer division
```

### Type annotations

Though it is not mandatory, it is best practice to always write explicit type declarations. The annotation is a form of documentation, which is verified by the compiler. They provide clear information as to how many arguments a function takes, what their types are, what order to pass them, and what the return type is. In the early stages of a program, type annotations help you think about what the function should be doing. In the long run, they improve the reliability, clarity, and scalability of your programs.

```elm
add : Int -> Int -> Int
add x y = x + y
```

Any type annotation must include a `:` and the type of the value returned. The `:` reads as "has type of". Types always begin with a capital letter (or open parenthesis). Note. Whenever a capital letter is followed by a dot, it's a module, not a type. `String.length` means the `length` function in the `String` module.

With functions that accept arguments, the argument types are added before the result value type. Each argument type is followed by a `->` that can be read as “returns”. The return type is the last item in the declaration, and the others as arguments. The pattern is then `function name : 1st arg type -> 2nd arg type -> return type`. Later on we'll see why they're all just separated with `->` instead of having some more explicit distinction between the return types and the parameters.

#### Basics

```elm
answer : Int
answer = 42
```

This reads as `answer` has type Int; answer equals "forty-two". (source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md)))

```elm
greeting : String -> String
```

This tells us that `greeting` is a function that accepts a String as argument and returns a String. (source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

```elm
double : Int -> Int
double x = x * 2
```

```elm
connectWords : String -> String -> String
connectWords firstWord secondWord =
  firstWord ++ secondWord
```

This says that `connectWords` is a function that accepts two strings and returns a string. If we look at the function we see that it indeed takes two parameters, firstWord and secondWord, and concatenates them. (source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

```elm
mult : Int -> Int -> Int
mult x y = x * y
```

#### Getting acquainted

A good way to familiarize yourself with type annotations is to browse through official or user contributed packages and check the annotations. This will help you develop a feel for them.

```elm
not : Bool -> Bool
round : Float -> Int
```

Use elm-repl to examine the types of some expressions. We'll do that by typing expressions into elm-repl. After each valid expression, the repl tells us its type.

If you want to give your function a type declaration but are unsure as to what it should be, you can always just write the function without it and then check it in elm-repl. Functions are expressions too, so it works on them without a problem. To write a multi-line expression in elm-repl, add a backslash to the end of each line you want to continue on the line below,
like this. (source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

```elm
> addThree x y z = \
|     x + y + z
<function> : number -> number -> number -> number
```

#### -> "goes to" curried function

The key to understanding type annotation in Elm is to acknowledge that at the heart of any functional programming language, we have functions that can return functions. 

A special case is currying. A function takes a single argument and returns a function that takes the next argument, etc. When all arguments have been taken, a result is computed and returned. 

```elm
repeatString: Int -> String -> String
-- takeQty qty = (\ text fn -> fn(qty))
-- takeText txt = takeQty text String.repeat
```

// type a-goes-to-b, goes to list of a, goes to list of b

----

`->` is used to separate the arguments and return value in the types of functions. It's pronounced "goes to". So for example, `String.length : String -> Int` is pronounced "string-dot-length has type String goes to Int". You just read left to right, like an English sentence.

What's really going on is that the type annotation is telling you about *partial application*: you can give a function only some of its arguments, and get a function as a result. You can always get the new function's type annotation by covering up part of the left side of the original function's annotation.

```elm
update : Action -> Model -> Model
-- implied parentheses in the annotation.
update' : Action -> (Model -> Model)
```

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))


#### Parametrised types

With collections like lists, tuples, records, annotations take a slightly different form. They specify both the type of the collection and the type of value held in the collection. 

List:
```elm
flagColors: List String
flagColors = ["Black", "Yellow", "Red"]
diceFaces : List Int
diceFaces = [1,2,3,4,5,6]
```

Tuple:
```elm
answerToEverything : (String, Int, Bool)
answerToEverything = ("the answer", 42, True)
```

Record:
```elm
startPosition : { x : Float, y : Float }
startPosition =
    { x = 0,
      y = 0
    }    
```

#### Type variables

Functions don't necessarily need to be aware of the type of values held in a collection to computer an answer. Take `List.length`. It doesn't really matter whether the values inside the list are all strings, numbers, or complex records. What we are after is the number of items in the list.

Type variables help with this. Check the `List.length` signature using the REPL.

```elm
$ elm repl
> List.length
<function> : List a -> Int
```

The lower case letter `a` is what is known as a type variable. It is a placeholder for a specific type that is left unspecified. The only requirement for running `List.length` is a List with values of the same type. It can be any type. The function returns the length of the list, represented as a `Int`.

```elm
$ elm repl
> List.take
<function> : Int -> List a -> List a
List.take 2 [1,2,3,4] -- [1,2]
List.take 2 ["a","b","c"] -- ["a","b"]
```

A given type variable can appear multiple times in the annotation. It then means that whatever type `a` holds, `a` must always refer to that exact same type. In the case of `List.take`, if the initial List had values of type `Int`, then the resulting list will have values of type `Int`.

Some functions can accept, as arguments, values of different types. The convention is to use single letters starting at the beginning of the alphabet, `a, b, c, d`, to mark each new value type. An example is the `List.unzip` function, which decomposes a list of tuples into a tuple of lists.

```elm
$ elm repl
> List.unzip
<function> : List ( a, b ) -> ( List a, List b )
List.unzip [(0, True), (17, False), (1337, True)] -- ([0,17,1337], [True,False,True])
```

Here both `a` and `b` are introduced, to specify that the function works for any types, provided that all occurrences of `a` resolve to the same type and all occurrences of `b` resolve to the same type. 

Another example is the `fst` function that returns the first element of a tuple.

```elm
$ elm repl
> fst
<function> : ( a, b ) -> a
> fst ("Marc", 21) -- "Marc"
> fst ("John", "Doe") -- "Marc"
```

The function `fst` takes a pair expressed as a tuple and returns a value that is of the same type as the first element. The tuple is expressed as `( a, b )` because the value type for the first element can differ from the value type of the second. Two variable names`a` and `b` are introduced because the first and second values can be of different type variables. However, they don't have to be different types. The return value's type is the same as the one of the first element in the tuple. 

It is rare for functions to have more than 2 type variables. It however is not impossible. 

```elm
$ elm repl
> List.map5
<function:map5>
    : (a -> b -> c -> d -> e -> f)
      -> List a -> List b -> List c -> List d -> List e -> List f
```

Though the convention is to use `a,b,c,d`, (almost) any lowercase string will work. Sometimes, another letter or a descriptive word, can help better capture the intent. For example, `Dict k v` reminds us that the types variables are the keys and values.


#### Constrained Type variables

Elm has three special types of type variables: `number`, `comparable`, and `appendable`.

A `number` is any type that supports basic arithmetic (except division, which is handled separately for each type). These are `Int` or `Float`.

```elm
doubleMe : number -> number
doubleMe x =
    x + x
```

* Arithmetic expressions: `+`,`-`, `*`, `/` (result is always a float), `//` (integer division), `%` (mod), parentheses

A `comparable` represents types that can be compared or ordered, like 1 == 1 or 1 < 2. This can be `String`, `Char`, `Int`, `Float`, `Time`, or a `List` or `tuple` containing only comparable values. Surprisingly enough, comparables can be compared with operations like `(>)`. Elm's dictionaries and sets are implemented as binary search trees, so the keys or elements must be comparable. 

* Comparators: `<`, `<=`, `==`, `!=`, `>=`, `>`


An `appendable` represents items that can be appended to each other with the `++` operator. These can be `string`, `text` (i.e. with typesetting information), or a `list` (containing any type). 

To use any of these types, just use their name in an annotation instead of a specific type or type variable.

If one of these types appears multiple times in a type annotation, all occurrences must resolve to the same type. You can allow them to be different by sticking something on to the end of the type, like `appendable2` or similar. For example, if you enter `(4, 2)` into the Elm REPL, it will infer the type `(number, number')`. The apostrophe indicates that the second number need not be the same type as the first.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md) and [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))



### Grouping

The example given for `List.map5` highlight yet another pattern, grouping. It shows in all variants of `List.map`.

```elm
$ elm repl
> List.map
<function> : (a -> b) -> List a -> List b
```

`List.map` is a function that takes a function that converts a value of type `a` to a value of type `b`, then take a list of values of type `a`, and returns a list of values of type `b`. Type variables are used `(a -> b)` as the map function doesn't need to know what is in the list. It traverses the list and an apply a function to each value in the list, resulting in a new list. Only the function applied to each element needs to know what type those elements are. 

### Further Reading

- Types chapter in the Elm guide by Elm lang org
- Professor Frisby's Mostly Adequate Guide to Functional Programming: Chapter 4 - Currying by Brian Lonsdorf (dr boolean)
- Partial Function Application for Humans by Andrew Berls
- [Understanding the elm type system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))


