**Elm 0.17**, **shareable**

## Type System

In Elm every variable or function has a type, and this type never changes. A type is a kind of label that every expression has. It tells us in which category of things that expression fits.

Here is a quick overview of types available by default in any Elm program. They are described each in greater details in the [type section](../type/cover.md).

Primitives:
* Int: `2`     -- integers, limited to some range
* Float: `2.3` -- floats, or numbers with decimals
* Bool: `True` -- logical true and false
* Strings: `"hello"` -- multicharacter strings
* Chars: `'a'` -- Unicode code points or ASCII characters
* Named Functions: `isNegative n = n < 0`, `avg x y = (x+y)/2`
* Anonymous functions:`(\ x y -> (x + y) / 2)`

Constrained types
* number: `2 + 2.3 -- 4.3 : Float`
* comparable: `3 > 2 -- True : Bool`
* appendable: `"hello" ++ " elm" -- "hello elm" : String`

Collections:
* Lists: `[1, 2] or ["one","two","three"]` -- all elements must have the same type
* Tuples: `("Dave",True) ` -- fixed number of values, any mix of types
* Records: `{x = 0,y = 10}` -- key-value pairs

Special types
* Aliases: `type alias Point={x=0,y=0}`
* Union type: `type Msg = Decrement | Increment`
* Unit type: `()`
* Maybe type
* Result type

### Strong / Static type system

Elm is statically typed. A static type system means that the type of every expression is known at compile time and any kind of error related to data types can be detected before compiling it. Types of all functions and variables must be correct for the program to compile. All errors must be fixed before the program can run.

In case of error, the compiler informs that there is a problem and compilation is aborted. The error messages are generally very informative. For instance, if you write a program where you try to add a integer type with some string type, Elm will let us know that `+` expects its left and right side to be numbers.

```elm
$ elm repl
> 2 + "three"
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm

The right argument of (+) is causing a type mismatch.

3|   2 + "3"
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

### Type inference

We don't have to explicitly write out the types of our functions and expressions. Elm, like most Functional languages, Elm has type inference. The compiler will automatically infer the type of every value in your program.

```bash
$ elm repl
> 2 + 3
5 : number
> 2 + (3 / 1)
5 : Float
> 2 + (round 3)
5 : Int
```

Both `2` and `3` are recognized as number literals. Without a decimal point they can be treated as Int or Float, depending on the operations run on them.

### Type annotations

Though it is not mandatory, when defining values or functions, it is best practice to include the type signature as part of the definition. The annotation provide clear information as to how many arguments a function takes, what their types are, what order to pass them, and what the return type is.

Elm expresses type with this notation:

```elm
f : X -> Y
```

It asserts that f is a function taking arguments of type X and returning results of type Y.

In the early stages of a program, type annotations help you think about what the function should be doing. In the long run, they provide documentation verified by the compiler and improve the reliability, clarity, and scalability of your programs.

// As you program in Elm, you follow a delicious breadcrumb trail of extremely readable compiler error messages until the program compiles and everything works. By leveraging the type system in Elm, you account for all scenarios before you even open a browser. You never open the JavaScript console. // For example, when writing an Elm program I might at some point decide that users should have an admin flag. I will then try to use that flag in a function at which point the compiler will tell me that I have failed to add it to the User model. I will add it to the model at which point the compiler will tell me that I have failed to account for it in my main update function. // You really have to try Elm yourself to get a feel for the power of this idea and the way it guides your programming style. (source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

#### Annotation shape

```elm
add : Int -> Int -> Int
add x y = x + y
sin : Float -> Float
add : (Int,Int) -> Integer
logBase : Float -> (Float -> Float)
```

Any type annotation must include a `:` and the type of the value returned. The `:` reads as "has type of". Types always begin with a capital letter (or open parenthesis). Note. Whenever a capital letter is followed by a dot, it's a module, not a type. `String.length` means the `length` function in the `String` module.

With functions that accept arguments, the argument types are added before the result value type. Each argument type is followed by a `->` that can be read as “returns”. The return type is the last item in the declaration, and the others as arguments. The pattern is then `function name : 1st arg type -> 2nd arg type -> return type`. Later on we'll see why they're all just separated with `->` instead of having some more explicit distinction between the return types and the parameters.

#### Variants

```elm
answer : Int
answer = 42

greeting : String -> String
greeting x = "Hello " ++ xx

double : Int -> Int
double x = x * 2

connectWords : String -> String -> String
connectWords firstWord secondWord =
    firstWord ++ secondWord

mult : Int -> Int -> Int
mult x y = x * y
```

#### Getting acquainted

The easiest way to explore type expressions by browsing through the elm-lang official libraries or typing expressions in the Elm REPL tool.

Try common functions:

```elm
$ elm repl
> not
<function:not> : Bool -> Bool
> round
<function:round> : Float -> Int
```

Or write new functions and check their type in the Elm REPL:

```elm
$ elm repl
> fun x y = "" ++ x ++ y
<function> : String -> String -> String
> square x = x*x
<function> : number -> number
> (\x -> x*x)
<function> : number -> number
-- import code from a core library
> import List
> List.map
<function> : (a -> b) -> List a -> List b
-- import code from a file
> import MyModule.elm
> MyModule.myFunction
```

You can read more about the REPL in [Local REPL](https://widged.gitbooks.io/gb-elm-platform/content/en/tools/02-repl.html)


#### -> "goes to" curried function

The symbol `->` separates the arguments and return value in the types of functions. It reads as  "goes-to". The signature `map : (a->b) -> List a -> List b` would read as "map has type type a-goes-to-b, goes to list of a, goes to list of b".

The key to understanding type annotation in Elm is to be aware that all functions in Elm are _curried_ by default. If you have a function with multiple arguments, it first takes one argument and returns a function that takes the next argument(s), etc. When all arguments have been taken, a result is computed and returned.

That's why function arguments are not wrapped between parentheses, like they would in imperative languages. If we were to use parentheses in Elm, we woudl write `someFunction arg1 arg2` as `((someFunction arg1) arg2)`.

```elm
concat : String -> String -> String
concat a b = a ++ b
-- is equal to
concat a = (\b -> a ++ b)
-- is equivalent to
concat = \a -> (\b -> a ++ b)
```

#### -> is right associative

The arrow `-> `in type annotations of functions is right-associative. In other words, a -> List a -> Bool is the same as a -> (List a -> Bool).

#### Because of currying variables can be omitted

Any of these two can be written.

```elm
double : List number -> List number
double lst  = List.map (\x -> x * 2) lst
doubled = double [1,2,3]

-- The `lst` variable is in the end of both parameter lists, so you can just omit it.
double : List number -> List number
double  = List.map (\x -> x * 2)
doubled = double [1,2,3]
```

### Constrained Type variables

Elm has three special types of type variables: `number`, `comparable`, and `appendable`.

A `number` is any type that supports basic arithmetic (`+`,`-`, `*`; except division, which is handled separately for each type: `/` always results a float, `//` for integer division), `%` for mod). These are `Int` or `Float`.

```elm
$ elm repl
> doubleMe : number -> number
> doubleMe x = x + x
```

A `comparable` represents types that can be compared or ordered, like `1 == 1` or `1 < 2`. This can be `String`, `Char`, `Int`, `Float`, `Time`, or a `List` or `tuple` containing only comparable values. Surprisingly enough, comparables can be compared with operations like `<`, `<=`, `==`, `!=`, `>=`, `>`.

An `appendable` represents items that can be appended to each other with the `++` operator. These can be `string`, `text` (i.e. with typesetting information), or a `list` (containing any type).

To use any of these types, just use their name in an annotation instead of a specific type or type variable.

If one of these types appears multiple times in a type annotation, all occurrences must resolve to the same type. You can allow them to be different by sticking something on to the end of the type, like `appendable2` or similar. For example, if you enter `(4, 2)` into the Elm REPL, it will infer the type `(number, number')`. The apostrophe indicates that the second number need not be the same type as the first.


### Primary Data Structures

* [Lists](../type/03-list.md)
* [Tuple](../type/04-tuple.md)
* [Record](../type/05-record.md)

#### Type variables

If look at the signature of the basic tuple, list, record data structures, you will see lower case letters appear.

```elm
$ elm repl
> -- tuples
> fst
<function> : ( a, b ) -> a
> snd
<function> : ( a, b ) -> b
> -- lists
> import List
> List.map
<function> : (a -> b) -> List a -> List b
> List.length
<function> : List a -> Int
> -- record
> .name
<function> : { b | name : a } -> a
```

These are called type variables. They stand for a value type that the function doesn't necessarily need to be fully specified for an answer to be computed. Take `List.length`. It doesn't really matter whether the values inside the list are all strings, numbers, or complex records. What we are after is the number of items in the list.

The lower case letters like `a` or `b` in the type signature are known as type variables. Each letter is a placeholder for a specific type that is left unspecified. It indicates that the function works for any type. A given type variable can appear multiple times in the annotation. In this case, whatever type `a` holds, all occurrences of `a` resolve to the same type

Take the `identity` function. It takes a value and returns that value. The types are de facto identical.

```elm
$ elm repl
> identity
<function> : a -> a
> identity 8
8 : number
> identity "a"
"a" : String
> identity {name = "John"}
{ name = "John" } : { name : String }
```

Similarly, the List.map signature `(a -> b) -> List a -> List b` says that this function takes as first parameter a function that transforms values of type a into value of type b and as second parameter a List of elements of type a and returns a list of values of type b.

Another example is the `List.unzip` function, which decomposes a list of tuples into a tuple of lists.

```elm
$ elm repl
> List.unzip
<function> : List ( a, b ) -> ( List a, List b )
List.unzip [(0, True), (17, False), (1337, True)] -- ([0,17,1337], [True,False,True])
```

A new letter is used anytime a new argument is introduced that can hold a different type. The convention is to use single letters starting at the beginning of the alphabet, `a, b, c, d`, to mark each new value type.  It is rare for functions to have more than 2 type variables. It however is not impossible.

```elm
$ elm repl
> List.map5
<function:map5>
    : (a -> b -> c -> d -> e -> f)
      -> List a -> List b -> List c -> List d -> List e -> List f
```

Though the convention is to use `a,b,c,d`, (almost) any lowercase string will work. Sometimes, another letter or a descriptive word, can help better capture the intent. For example, `Dict k v` reminds us that the types variables are the keys and values.

### Further Reading

* Types chapter in the Elm guide by Elm lang org
* Professor Frisby's Mostly Adequate Guide to Functional Programming: Chapter 4 - Currying by Brian Lonsdorf (dr boolean)
* Partial Function Application for Humans by Andrew Berls
* [Understanding the elm type system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html)
* [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md)
* [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md)
