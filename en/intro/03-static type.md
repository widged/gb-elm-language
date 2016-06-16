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
* Strings: `"hello"`. 
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

Custom types defined in official or community contributed packages.

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

## Type inference

The Elm compiler has type inference and can figure things out. (check it out using the REPL)

However, it is best practice to always add type annotations. Type annotations improve code by helping you think about what the function should be doing, and serve as compiler-verified documentation. (You know how an out-of-date comment is worse than no comment at all? Well, type annotations never get out of date.) In addition, if you ever want to publish a third-party library, you will need type annotations.

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))



## Type annotations

```elm
add : Int -> Int -> Int
add x y =
  x + y
```

Getting started with static typing can be difficult. Everything seems to go a little bit slower and take a little more effort before you see results. But don’t worry about it! You will find that Elm has a very approachable syntax for working with types and that using them will improve the reliability and clarity of your progams.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

Adding annotations to our functions to make the typing more clear. They  specify how many arguments a function takes, what their types are, what order to pass them, and what the return type is.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md]])

By paying attention to Type annotations and understanding their syntax, you will write programs that are reliable, scalable, and easy to understand. When you hit tough spots while learning, don’t be shy about exploring functional programming concepts (like partial application and currying) to help unlock the bigger picture behind Elm’s design.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

Any type annotation must include a `:`. The `:` means "has type". It is followed by the type of the value returned. Types always begin with a capital letter (or open paren).

(source: [elm-for-js](https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md))

## Type Annotation

Elm, like most ML dialects, automatically infers most types.

```elm
-- function name : 1st arg type -> 2nd arg type -> return type
fnc : Int -> List -> Int
```
Example below is read as function that takes an __a__ value and returns a __b__ value, list of __a__ values returns a list of __b__ values

```elm
map: (a -> b) -> List a -> List b
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

Pattern matching on record fields
```elm
-- Requires the argument with x and y fields
multiply {x,y} =
    x * y
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))

Annotating records
```elm
coordinates : { x : Float, y : Float }
coordinates =
    { x = 0,
      y = 0
    }    
```

(source: [learnyouanelm](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/02-starting-out.md))


### No parameters. Just return value

```elm
answer : Int
answer = 42
```

You can read "answer has type Int; answer equals forty-two".

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md]])


### One parameter and a return value


When the function accepts on or more arguments, the argument types are added before the result value type and each followed by a `->` that can be read as “returns”. For instance, for a single argument, we would have:

    greeting : String -> String

What this says is that `greeting` is a function that accepts a String as argument and returns a String.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

### Two parameters and a return value


When the function accepts multiple arguments, they are all added as a succession of `{Type} ->`:

    connectWords : String -> String -> String
    connectWords firstWord secondWord =
      firstWord ++ secondWord

Here, connectWords is a function that accepts two strings and returns a string. If we look at the function we see that it indeed takes two parameters, firstWord and secondWord, and concatenates them.

(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])

### -- extras

#### Types and Type Annotations

Type annotations aren't for the compiler (it has type inference and can figure things out) but for you, the programmer.

The annotation tells you how many arguments a function takes, what their types are, what order to pass them, and what the return type is. This information is not repeated elsewhere.

Once you know how to read an annotation, it's fairly easy to write them. Doing so is is optional, but highly encouraged. Type annotations improve code by helping you think about what the function should be doing, and serve as compiler-verified documentation. (You know how an out-of-date comment is worse than no comment at all? Well, type annotations never get out of date.) In addition, if you ever want to publish a third-party library, you will need type annotations.


(source: ???)

##### Definitions

The first thing to know is that : means "has type".

~~~~ {.Elm:hs name="code"}
answer : Int
answer = 42
~~~~

You can read "answer has type `Int`; answer equals forty-two".

Common primitive types include `Int` , `Float` , `Bool` , and `String` . You can also pair up types into tuples, for example (`Int`, `Bool`) . This expands to arbitrarily many elements, i.e. (`Int`, `Float`, `Int`) is a 3-tuple with first element `Int` , second `Float` , third `Int` .

~~~~ {.Elm:hs name="code"}
myTuple : (String, Int, Bool)
myTuple = ("the answer", 42, True)
~~~~

The compiler will infer the type of every value in your program.
Types always begin with a capital letter (or open paren).
Some common types, which you might see in Elm's REPL.

~~~~ {.Elm:hs name="code"}
5 : Int
6.7 : Float
"hello" : String
True : Bool
~~~~

There's one special type with only one value of that type. Both the type and value are read as "unit" and written as () . Unit is often used as a placeholder value, since if we know the type, we can anticipate the value.

Note. Whenever a capital letter is followed by a dot, it's a module, not a type. `String.length` means the `length` function in the `String` module.

(source: ???)


#### Types for Functions

Functions have types too. -> is used to separate the arguments and return value in the types of functions. It reads as "goes to". So for example, String.length : String -> Int is pronounced "string-dot-length has type String goes to Int".

Think of the rightmost type as the type of the return value, and the others as arguments.


~~~~ {.Elm:hs name="code"}
not : Bool -> Bool
round : Float -> Int
~~~~

When you define a value, it's good practice to write its type above it.
The annotation is a form of documentation, which is verified by the compiler.
~~~~ {.Elm:hs name="code"}
double : Int -> Int
double x = x * 2
~~~~

Things get interesting with multiple arrows. "mult has type `Int` goes to `Int` goes to `Int`."

~~~~ {.Elm:hs name="code"}
mult : Int -> Int -> Int
mult x y = x * y
~~~~

Function arguments are passed in parentheses.

Lowercase types are type variables: they can be any type, as long as each call is consistent.
~~~~ {.Elm:hs name="code"}
List.map : (a -> b) -> List a -> List b
~~~~

Things get interesting with multiple arrows.


"List dot map has type a-goes-to-b, goes to list of a, goes to list of b."

(source: ???)


## Understand partial function application

The key to understanding type annotation in Elm is to realize that every function is curried by default.

    connectWords : String -> String -> String

As Professor Frisby’s Mostly Adequate Guide to Functional Programming puts it: “You can call a function with fewer arguments than it expects. It returns a function that takes the remaining arguments.”

So if I have a type annotation of a -> b -> c, I can pass a into the function and it will return a function b -> c. If I pass b into that function, then it will return the value c.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))


## Type Variables

If you look at the List library, this isn't actually how [List.map](http://package.elm-lang.org/packages/elm-lang/core/latest/List#map) is defined. Instead, it has lowercase type names, which are *type variables*:

```elm
List.map : (a -> b) -> List a -> List b
```

This means that the function works for any types `a` and `b`, as long as we've fixed their values, which usually happens by passing arguments whose types we know. So we could give it a `(Float -> Int)` and a `List Float`, or we could give a `(String -> Action)` and a `List String`, and so on. (This use of "variable" is closer to algebra than JavaScript, in that it's something you or the compiler find based on constraints, not explicitly set to whatever you need it to be.)

By convention, type variables are single letters starting at the beginning of the alphabet, although (almost) any lowercase string will work. Occasionally it's helpful to use another letter or a descriptive word, especially if you have more than one type variable. For example, `Dict k v` reminds us that the types variables are the keys and values. It's possible for a type to have any number of type variables, but more than two is rare.

Type variables let us write generic code, like lists and other containers that can hold any type of value. Each particular container can only hold one type, but you get to pick what that is.  Then `List.map` can traverse a list and apply a function to it, without knowing what's in the list. Only the function applied to each element needs to know what type those elements are.

If `List a` is a list of any type, what is just `List`? Technically it's called a *type constructor*, but the better answer is that it's not really anything. It can't really exist on its own. The best way to think of it is that `List a` is the base type, and sometimes the type variable `a` gets replaced with a real type.

(source: [[https://github.com/elm-guides/elm-for-js/blob/master/How%20to%20Read%20a%20Type%20Annotation.md]])


a and b are what’s known as type variables. They are placeholders for any type. So in this example, a function that accepts a value of type a really means a function that accepts a value of any type. The important thing is that type a stays the same and type b stays the same. Here are some possiblities for the map function:

    -- accepts:
    -- a function which accepts an Int and returns a String
    -- a List of Integers
    -- returns:
    -- a List of Strings
    map : (Int -> String) -> List Int -> List String

    -- accepts:
    -- a function which accepts a String and returns a Bool
    -- a List of Strings
    -- returns:
    -- a List of Booleans
    map : (String -> Bool) -> List String -> List Bool
    Here is an example that would violate the type annotation (think of it as the contract) for the map function:

    -- map : (a -> b) -> List a -> List b
    map : (Int -> String) -> List Int -> List Int

This situation would never happen because the type variable b is not matching. If map received a function that accepted Integers and returned Strings as its first argument, we have a guarantee that it will always return a List of Strings.

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

## Type variables

What do you think is the type of the List `length` function? Because `length` takes a list of any type and returns either `Just` the first element or `Nothing`, so what could it be? Let's check!

```elm
repl> List.length
<function> : List a -> Int
```

Hmmm! What is this `a`? Is it a type? Remember that we previously stated that types are written in capital case, so it can't exactly be a type. Because it's not in capital case it's actually a *type variable*. That means that `a` can be of any type. This is much like generics in other languages, only in Elm it's much more powerful because it allows us to easily write very general functions if they don't use any specific behavior of the types in them. Functions that have type variables are called *polymorphic functions*. The type declaration of `length` states that it takes a list of any type and returns an `Int` representing the length of that list.

Although type variables can have names longer than one character, we usually give them names of a, b, c, d …


(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

If you look at the List library,  [List.map](http://package.elm-lang.org/packages/elm-lang/core/latest/List#map) is defined as.

```elm
List.map : (a -> b) -> List a -> List b
```

It has lowercase type names, which are *type variables*. This means that the function works for any types `a` and `b`, provided that all occurrences of `a` resolve to the same type and all occurrences of `b` resolve to the same type. `List a` shows that a list can only hold one type, but you get to pick what that is. Then `List.map` can traverse a list and apply a function to it, without knowing what's in the list. Only the function applied to each element needs to know what type those elements are.

By convention, type variables are single letters starting at the beginning of the alphabet, although (almost) any lowercase string will work. Occasionally it's helpful to use another letter or a descriptive word, especially if you have more than one type variable. For example, `Dict k v` reminds us that the types variables are the keys and values.

It's possible for a type to have any number of type variables, but more than two is rare.

```elm
show 4 == 5 -- False
show "Hello" ++ "!" -- "Hello!"
```
Remember `fst`? It returns the first component of a pair. Let's examine its type.

```elm
repl> fst
<function> : ( a, b ) -> a
```

We see that `fst` takes a tuple which contains two types and returns an element which is of the same type as the pair's first component. That's why we can use `fst` on a pair that contains any two types. Note that just because `a` and `b` are different type variables, they don't have to be different types. It just states that the first component's type and the return value's type are the same.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))

Note. `List` is called a *type constructor*. However, It can't really exist on its own.

(source: [learnyouanelm-03](https://github.com/learnyouanelm/learnyouanelm.github.io/blob/master/pages/03-types.md))



## Grouping

Here is an example of a more complicated type annotation:

    map : (a -> b) -> List a -> List b

There are two things that stand out in this example: the use of parentheses, and the values a and b. The parentheses work the same way they work in math. In other words, they provide an order of operations for the compiler. What that means is that (a -> b) can be thought of as a complete function which accepts a value of type a and returns a value of type b. With that grouping we can read this type annotation as:

map is a function that accepts a function and a List and returns a List

(source: [[http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html]])



## Tips

Tip: Look at type annotations in the Elm docs as you learn. At first they will seem strange but you will quickly develop a feel for them and before long you will find yourself looking at the type annotations and not even looking at the details of the function!

(source: [understanding-the-elm-type-system](http://www.adamwaselnuk.com/elm/2016/05/27/understanding-the-elm-type-system.html))

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
