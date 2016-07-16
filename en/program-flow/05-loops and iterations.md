**Elm 0.17**, **shareable**

## Loops and Iterations

Looping in Elm works very differently than it does in mainstream languages. Typical constructs like `while` or `for` cannot work given that data is immutable.

Loops must be expressed using recursion. And it is important to understand how recursion works in Elm, because most of the complex code is based on this mechanism.

However, recursion can make it harder for beginners to grasp loop programs. Higher-levels abstractions are provided that hide the recursion details.

Any non-empty list is a recursive structure can be expressed in the form (head::tail).

```elm
sum : List number -> number
sum lst =
  case lst of
    [] ->   0
    (x::xs) -> x + sum(xs)

sum [1,4, 6, 8] -- 19
```

The sum function  at first sight only makes sense on non-empty list. However, you can assign the value 0 to the sum of an empty list, because if you add that value to any number it does not change. Values like 0, which can be safely applied with respect to an operation, like sum, are called the neutral elements of that operation.

### Higher-order functions for writing loops

Higher-order functions make writing loops much easier. There are many useful generic iteration functions in the List module.

Higher level of abstraction allow to write more efficient expressions.

```elm
sum : List number -> number
sum' lst = List.foldl (+) 0 lst

sum' [1,4, 6, 8] -- 19
```

The List package is a Swiss army knife for loops and iterations; it contains a lot of useful functions. You should spend some time researching the module documentation (http://package.elm-lang.org/packages/elm-lang/core/4.0.1/List).

With List.map, the code now is much more concise, and the actual intention of the programmer is explicit from the use of the map function.


### Tail recursion

Tail recursion is used when you need to run an arbitrarily long loop.
