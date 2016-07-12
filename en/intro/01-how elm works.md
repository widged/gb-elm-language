## How Elm Works

Elm is a powerful, fast, type-safe, functional programming language.

Elm's approach to programming can be summarized, as follows:

* Functional language (functions as value)
* Module system
* Strong / static typing
* No runtime exceptions
* Clean syntax (Haskell like)
* No null or undefined (Maybe and Result types to explicitly differentiate a null value from an undefined one).


Elm belongs to the family of *functional programming languages*. Generally speaking, functional programming is a method of program construction that emphasises functions and their application to their arguments (in contrast to imperative languages, which emphasizes the use of objects, and construct programs from executing commands and changing stored values).

Functions are first-class citizens. This means that functions can be manipulated as any other data in a program.

* Functions can be named: `square = \x -> x * x`
* Functions can be passed as argument to another function : `map square [1, 3, 5, 7]`
* Functions can produce functions as results: `addTwo = let add x = (\y -> x + y) in add 2`
* Functions can be stored in data structures such as lists:  `let mult x y = x * y in List.map2 (\f v -> f v) [mult 2, mult 3, mult 5] [1, 3, 5] -- [2,9,25]`

This ability to treat functions as data allows a higher level of abstraction. It promotes the heavy uses of higher-order functions and
the division of the code into many small, highly reusable, easily composable functions. This in turn helps write code that tends to display *simplicity, conciseness, and clarity*. Programs written with functional languages are often between two and ten times shorter than programs written in other current languages. For instance, a complex algorithm like quicksort, can be written in as little as 4 lines of code.

```elm
qsort lst =
  case lst of
    x:xs -> qsort (filter ((>=)x) xs) ++ [x] ++ qsort (filter ((<)x) xs)
    [] -> []
```

Functional languages also favour functions that do no cause any observable side effects. They are qualified as *pure* when they take all their input as arguments and produce all their output as results. They will not read or write any mutable variables, nor change the state of the application in any way.

Obviously, web applications require some form of side-effects. For instance, reacting to a user clicking on a button, making a request to a webservice, updating an embedded database, communicating through sockets. Elm, addresses this by providing a uniform framework for handling effects without compromising the purity of functions. The language forces the programmer to separate the pure, side-effect free, parts from the impure ones. Every boundary with the code that deal with side-effects is sanitized.

Function purity makes it hard to implement some traditional looping constructs. In pure laguages, *Recursion as the primary control structure*. Mechanisms of repetition and looping are typically achieved using functions that are defined in terms of themselves  and are hence recursive. This is demonstrated in the quicksort example.

The main benefit of purity is the improved ability to reason about the code. Being stateless, functions are guaranteed to return the same result when called with the same data as arguments. This property is called referential transparency, and it’s the ground for the application of formal verification techniques. // That make them functions in the mathematical sense and this simple mathematical basis provides support for equational reasoning, the ability to reason about the properties of programs, using a very simple process of computation by calculation. We can reason about programs using not just use properties of built-in operations of the language such as addition and multiplication, but also use the equations from which user-defined functions are constructed.

This allows for a *Strong, statically checked, type system*. Every function has a type that specifies the nature of its arguments and results. Type information can be specified by the programmer, however, the type of a function arguments and results can be automatically inferred from the definition of the function in the absence of any explicit type information. For instance, if you say `a = 5 + 4`, you don't have to tell Elm that `a` is a number, it can figure that out by itself.

An important advantage of a strong type system is that programs can be statically checked at compile time. For each expression, the type (inferred or supplied by the programmer) is checked. Any incompatibility errors in between the type of actual arguments and the expected type of an expression will be automatically detected. For example, attempting to add a number and a character would be reported as an error, because characters are not a type of numbers. Checks are not limited to types. They encompass any branching condition, to help guarantee we’ve accounted for every possible branching case. The programs won't compile until all errors have been fixed. Hence, the saying “once it compiles, it works”.

Sure, compiler errors can be frustrating when learning the language. However, Elm's compiler does it best to provide readable compiler messages. Catching bugs early is cheaper than generaring inccorect code, running it, getting users report an issue, debugging the code, and gradually tracing the problem back to its original cause. As programs are more maintainable and less error-prone, refactoring also becomes easier. It helps develop larger and faster applications.

In most functional programming languages, purity is an unescapable requirement, coming from a commitment to lazy evaluation, which is based upon the idea that no computation should be performed until its result is actually required (call-by-need).  If laziness forces the language to remain pure, purity doesn't depend on a lazy evaluation model. Elm opted for evaluation model is based on eagerness (call-by-value) [Also better suited to streaming]


### trying out

At this point you must be feeling the need to actually try Elm on your own computer. The first step for this is, of course, to have a working Elm installation in your system.

See [gb-elm-platform](https://www.gitbook.com/book/widged/gb-elm-platform/details)
