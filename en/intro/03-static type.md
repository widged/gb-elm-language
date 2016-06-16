**Elm 0.??**, **copied, please ignore** 


# Strong / Static type system

The goal of a type system is to prevent any kind of error related to data types. This provides high level of confidence that your program will work as expected.

Static typing means that the actual source code (the text file) of your Elm program is verified by a compiler. The compiler looks for type related errors and, if it finds one, it tells you loud and clear that there is a problem and refuses to build the program. All errors must be fixed before the program can run. This is why Elm is said to have “no runtime exceptions”.

(adapted from [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

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


## Types

Here is a quick overview of types available by default in any Elm program. They are described each in greater details in the type section. 

Primitives:
* Int: `2` 
* Float: `2.3`
* Bool: `True`
* Strings: `"hello"`
* Chars: `'a'`
* Named Functions: `isNegative n = n < 0`, `avg x y = (x+y)/2`
* Anonymous functions:`(\ x y -> (x + y) / 2)`

Collections:
* Lists (all elements must have the same type): `["one","two","three"]`
* Tuples (fixed number of values, any mix of types): `("Dave",True) `
* Records (key-value pairs): `{x = 0,y = 10}`

Special types
* Aliases: `type alias Point={x=0,y=0}`
* Union type: `type Msg = Decrement | Increment`

Constrained types
* number: `2 + 2.3 -- 4.3 : Float`
* comparable: `3 > 2 -- True : Bool`
* appendable: `"hello" ++ " elm" -- "hello elm" : String`


## Type inference

Elm, like most ML dialects, has type inference. The compiler will automatically infer the type of every value in your program.

```bash
$ elm repl
> 2 + 2.3
4.3 : Float
```

Both `2` and `2.3` as identified as numbers. A Float is required to represent the answer. 

## Type annotations

Though it is not mandatory, it is best practice to always add type annotations. The annotation is a form of documentation, which is verified by the compiler. They provide clear information as to how many arguments a function takes, what their types are, what order to pass them, and what the return type is. In the early stages of a program, type annotations help you think about what the function should be doing. In the long run, they improve the reliability, clarity, and scalability of your programs.

```elm
add : Int -> Int -> Int
add x y = x + y
```

Any type annotation must include a `:` and the type of the value returned. The `:` means "has type". Types always begin with a capital letter (or open parenthesis). Note. Whenever a capital letter is followed by a dot, it's a module, not a type. `String.length` means the `length` function in the `String` module.

With functions that accept arguments, the argument types are added before the result value type. Each argument type is followed by a `->` that can be read as “returns”. Think of the rightmost type as the type of the return value, and the others as arguments. The pattern is then `function name : 1st arg type -> 2nd arg type -> return type`

### Basics

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

### Getting acquainted

A good way to familiarize yourself with type annotations is to browse through official or user contributed packages and check the annotations. This will help you develop a feel for them.

```elm
not : Bool -> Bool
round : Float -> Int
```

### -> "goes to" curried function

The key to understanding type annotation in Elm is to acknowledge that at the heart of any functional programming language, we have functions that make and return functions. 

A special case is currying. A function can be called with a single argument and return a function that takes the remaining arguments. With that in mind, a type annotation like `repeatString: Int -> String -> String` can be understood as taking a first argument (`Int`) and returning a curried function that takes the remaining arguments (`String`). When all arguments have been taken, the result is returned (`String`). 

### Collections need two things, collection type and value type

With collections like List, tuples, records, annotations take a slightly different form. They specify both the type of the collection and the type of value held in the collection. 

List:
```elm
flagColors: List String
flagColors: ["Black", "Yellow", "Red"]
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

## Type generalisation with type variables

Functions don't necessarily need to be aware of the type of values held in a collection to computer an answer. Take `List.length`. It doesn't really matter whether the values inside the list are ll strings, numbers, or complex records. What we are after is the number of items in the list.

Type variables help with this. Check the `List.length` signature using the REPL.

```elm
$ elm repl
> List.length
<function> : List a -> Int
```

The lower case letter `a` is what is known as a type variable. It is a placeholder for a specific type that is left unspecified. The only requirement is that all values in the list have the same type. It can be any type. The function returns the length of the list, represented as a `Int`.

```elm
$ elm repl
> List.take
<function> : Int -> List a -> List a
List.take 2 [1,2,3,4] -- [1,2]
List.take 2 ["a","b","c"] -- ["a","b"]
```

A given type variable can appear multiple times in the annotation. It then means that whatever type `a` holds, `a` must always refer to that exact same type. In the case of `List.take`, if the initial List had values of type `Int`, then the resulting list will have values of type `Int`.

Another example is the `fst` function thatreturns the first element of a tuple.

```elm
$ elm repl
> fst
<function> : ( a, b ) -> a
> fst ("Marc", 21) -- "Marc"
> fst ("John", "Doe") -- "Marc"
```

We see that `fst` takes a pair expressed as a tuple. `( a, b )` tell us that the value type for the first element can differ from the value type of the second (it doesn't have to be different; it can). The function returns a value that is of the same type as the first element.  Note that just because `a` and `b` are different type variables, they don't have to be different types. It just states that the first component's type and the return value's type are the same. 

For type variables, the convention is to use single letters starting at the beginning of the alphabet, `a, b, c, d`. However, (almost) any lowercase string will work. Sometimes, another letter or a descriptive word, can help better capture the intent. For example, `Dict k v` reminds us that the types variables are the keys and values.

Some functions require the use of different type variables. If both `a` and `b` are introduced, this means that the function works for any types, provided that all occurrences of `a` resolve to the same type and all occurrences of `b` resolve to the same type.

```elm
$ elm repl
> List.unzip -- Unzip decomposes a list of tuples into a tuple of lists.
<function> : List ( a, b ) -> ( List a, List b )
List.unzip [(0, True), (17, False), (1337, True)] -- ([0,17,1337], [True,False,True])
```

It is rare for functions to have more than 2 type variables. It however is not impossible. 

```elm
$ elm repl
> List.map5
<function:map5>
    : (a -> b -> c -> d -> e -> f)
      -> List a -> List b -> List c -> List d -> List e -> List f
```


## Grouping

Here is an example of a more complicated type annotation:

    map : (a -> b) -> List a -> List b

There are two things that stand out in this example: the use of parentheses, and the values a and b. The parentheses work the same way they work in math. In other words, they provide an order of operations for the compiler. What that means is that (a -> b) can be thought of as a complete function which accepts a value of type a and returns a value of type b. With that grouping we can read this type annotation as:

map is a function that accepts a function and a List and returns a List

(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])


Function arguments are passed in parentheses.

Lowercase types are type variables: they can be any type, as long as each call is consistent.
~~~~ {.Elm:hs name="code"}
List.map : (a -> b) -> List a -> List b
~~~~

Things get interesting with multiple arrows.


"List dot map has type a-goes-to-b, goes to list of a, goes to list of b."

(source: ???)

If you look at the List library,  [List.map](http://package.elm-lang.org/packages/elm-lang/core/latest/List#map) is defined as.

```elm
List.map : (a -> b) -> List a -> List b
```

Then `List.map` can traverse a list and apply a function to it, without knowing what's in the list. Only the function applied to each element needs to know what type those elements are.

(source: ???)

We could give it a `(Float -> Int)` and a `List Float`, or we could give a `(String -> Action)` and a `List String`, and so on. (This use of "variable" is closer to algebra than JavaScript, in that it's something you or the compiler find based on constraints, not explicitly set to whatever you need it to be.)




    -- accepts:
    -- a function which accepts an Int and returns a String
    map : (Int -> String) -> List Int -> List String

    -- accepts:
    -- a function which accepts a String and returns a Bool
    map : (String -> Bool) -> List String -> List Bool
    Here is an example that would violate the type annotation (think of it as the contract) for the map function:

    -- map : (a -> b) -> List a -> List b
    map : (Int -> String) -> List Int -> List Int

This situation would never happen because the type variable b is not matching. If map received a function that accepted Integers and returned Strings as its first argument, we have a guarantee that it will always return a List of Strings.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))


## More complicated patterns


Example below is read as function that takes an __a__ value and returns a __b__ value, list of __a__ values returns a list of __b__ values

```elm
map: (a -> b) -> List a -> List b
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Parameterised types

### Lists are parameterised types

Here is a list of strings, and a list of ints.
Notice their type declarations.

```elm
list1 : List String
list1 = [ "Eenie", "Meenie", "Miney", "Mo" ]

list2 : List Int
list2 = [ 10, 9, 8, 7 ]
```


(source: [elm-explained](https://github.com/niksilver/elm-explained))


### A simple example

When we declare a type, the left had side of the equals sign says
how it's used; the right hand side says how it's defined.

The following `MiscHolder` can hold any type of value. It's defined as an
unknown type `a` tagged with the `MiscHolder` tag (its right hand side),
but when we use it we must use both the `MiscHolder` tag and the
specific type that's in use at the time.

```elm
type MiscHolder a = MiscHolder a
```

Here's how we use it to define some values, each with its own
type declaration.

```elm
stringInABox : MiscHolder String
stringInABox = MiscHolder "Greetings, Earthlings"

floatInABox : MiscHolder Float
floatInABox = MiscHolder 45.1
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))


### When to use parentheses

Since `MiscHolder` has only one additional type, when we use it to
hold a List of Strings we have to put the `List String` in parentheses,
otherwise the compiler will report an error.

```elm
listOfStringsInABox : MiscHolder (List String)
listOfStringsInABox = MiscHolder ["Once", "upon", "a", "time"]
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))


### A more involved example

Let's define a type for something that's been weighed. We need
to capture the thing itself, the weight as a floating point number,
and the weighing units (expressed as a String). When we use `Weighed`
in a type declaration in future we need to use its name and
the type of thing that's been weighed.

First, some type definitions to get us started.

```elm
type Car = Car String
type AnimalPart = AnimalPart String
```

Now the main type definition.

```elm
type Weighed a = Weighed a Float String
```

Notice that the thing being weighed (which has unspecified type `a`)
has to have its type exposed on the left hand side of the equals
sign. Then when we declare the type of a `Weighed` thing we need to
specify the thing's type, too (a `Car` or an `AnimalPart`).

Now let's see some simple use...

```elm
volvo = Car "Volvo"
feather = AnimalPart "Ostrich feather"

volvoWeighed : Weighed Car
volvoWeighed = Weighed volvo 2260 "kg"

featherWeighed : Weighed AnimalPart
featherWeighed = Weighed feather 8.5 "g"
```

We can do it without the interim values, too, but if so we
have to use parentheses...

```elm
volvoWeighed2 : Weighed Car
volvoWeighed2 = Weighed (Car "Volvo") 2260 "kg"

featherWeighed2 : Weighed AnimalPart
featherWeighed2 = Weighed (AnimalPart "Ostrich feather") 8.5 "g"
```

If we were to put the code above into
[an Elm module called ParameterisedTypes](ParameterisedTypes.elm)
then here's how we might explore it in the Elm REPL:

```
> import ParameterisedTypes exposing (..)
> volvoWeighed
Weighed (Car "Volvo") 2260 "kg"
    : ParameterisedTypes.Weighed ParameterisedTypes.Car
> volvoWeighed == volvoWeighed2
True : Bool
> featherWeighed == featherWeighed2
True : Bool
>
```

(source: [elm-explained](https://github.com/niksilver/elm-explained))

# ------

# Believe the type

Previously we mentioned that Elm has a static type system. The type
of every expression is known at compile time, which leads to safer code.
If you write a program where you try to divide a boolean type with some
number, it won't even compile. That's good because it's better to catch
such errors at compile time instead of having your program crash.
Everything in Elm has a type, so the compiler can reason quite a lot
about your program before compiling it.

Unlike Java or Pascal, Elm has type inference. If we write a number,
we don't have to tell Elm it's a number. It can *infer* that on its
own, so we don't have to explicitly write out the types of our functions
and expressions to get things done. We covered some of the basics of
Elm with only a very superficial glance at types. However,
understanding the type system is a very important part of learning
Elm.

A type is a kind of label that every expression has. It tells us in
which category of things that expression fits. The expression `True` is a
boolean, `"hello"` is a string, etc.

Now we'll use elm-repl to examine the types of some expressions. We'll do
that by typing expressions into elm-repl. After each valid expression, the
repl tells us its type. Let's give it a whirl.

TODO: Add a section on elm-repl, or come up with a different way to accomplish
this in the online editor. Maybe type holes?

```elm
> 'a'
'a' : Char
> True
True : Bool
> [1, 2]
[1,2] : List number
> (True, 'a')
(True, 'a') : ( Bool, Char )
> 4 == 5
False : Bool
```

Here we see that typing an expression prints
out the value of that expression followed by `:` and its type.
`:` is read as "has type of". Explicit types are always denoted with the
first letter in capital case. `'a'`, as it would seem, has a type of `Char`.
It's not hard to conclude that it stands for *character*. `True` is of a
`Bool` type. That makes sense. Examining the type of
`[1, 2]` yields a `List number`. The lowercase `n` in the type of number indicates
a built-in type variable. We'll get to type variables later in this chapter.
A number can be either an `Int` or a `Float`, and will concretely become one or
the other depending on how it's used. Unlike lists, each tuple length has
its own type. So the expression of `(True, 'a')` has a type of `( Bool, Char )`,
whereas an expression such as `('a','b','c')` would have the type of
`( Char, Char, Char )`. `4 == 5` will always return `False`, so its type is `Bool`.

Functions also have types. When writing our own functions, we can choose
to give them an explicit type declaration. This is generally considered
to be good practice. From here on, we'll give all the functions that we
make type declarations. Remember the first function we made previously
that doubles a number? Here's how it looks like with a type declaration.

```elm
doubleMe : number -> number
doubleMe x =
    x + x
```
(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))


`doubleMe` has a type of `number -> number`, meaning that it maps
from a number to a number. That's because it takes one number as a
parameter and returns another as a result.
We didn't have to give this function a type declaration because
the compiler can infer by itself that it's a function from a number to a
number but we did anyway. But how do we write out the type of a function
that takes several parameters? Here's a simple function that takes three
integers and adds them together:

```elm
addThree : Int -> Int -> Int -> Int
addThree x y z =
    x + y + z
```

The parameters are separated with `->` and there's no special distinction
between the parameters and the return type. The return type is the last
item in the declaration and the parameters are the first three. Later on
we'll see why they're all just separated with `->` instead of having some
more explicit distinction between the return types and the parameters
like `Int, Int, Int -> Int` or something.

If you want to give your function a type declaration but are unsure as
to what it should be, you can always just write the function without it
and then check it in elm-repl. Functions are expressions too, so it works on
them without a problem. To write a multi-line expression in elm-repl, add a
backslash to the end of each line you want to continue on the line below,
like this.

```elm
> addThree x y z = \
|     x + y + z
<function> : number -> number -> number -> number
```
(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))





# Further Reading

- Types chapter in the Elm guide by Elm lang org
- Professor Frisby's Mostly Adequate Guide to Functional Programming: Chapter 4 - Currying by Brian Lonsdorf (dr boolean)
- Partial Function Application for Humans by Andrew Berls


(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])


Elm allows you to easily write very general functions if they don't use any specific behavior of the types in them. Functions that have type variables are called *polymorphic functions*. 
