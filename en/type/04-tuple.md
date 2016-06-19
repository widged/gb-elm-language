**Elm 0.17**, **copied, please ignore** 

## Tuples

Tuples are denoted with () and their values are separated by commas. They are used when you know in advance how many components some piece of data should have. The length is fixed. Unlike a list, , tuples don't have to be *homogenous*; their elements can be of different types.

```elm
$ elm repl
> ("elm", 42)
> ("Christopher", "Walken", 55)
```

### Type annotation

The type of a tuple records the number of components as well as the types of the components.

```elm
$ elm repl
> (1, "2", True)
(1,"2",True) : ( number, String, Bool )
```

In a program

```elm
userProfile : (String, String, Int)
userProfile = ("John", "Doe", 42)
```

The number of elements must be constant.  

```elm
$ elm repl
> [(1,2),(8,11),(4,5)]
[(1,2),(8,11),(4,5)] : List ( number, number' )
> [(1,2),(8,11,5),(4,5)]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
```
As must the type in any given position.

```elm
$ elm repl
> [(1,2),(8,"11"),(4,5)]
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The 1st and 2nd elements are different types of values.
> [(1,"2"),(8,"11"),(4,"5")]
[(1,"2"),(8,"11"),(4,"5")] : List ( number, String )
```

### Special cases

#### tuple of size one

```elm
$ elm repl
> (5)
5 : number
```

It doesn't make sense to have a tuple of size one as it stores the value it contains.


#### tuple with no value


```elm
$ elm repl
> ()
() : ()
> (,)
<function> : a -> b -> ( a, b )
> (,,)
<function> : a -> b -> c -> ( a, b, c )
```

`()` is called a unit type. See chapter on [Unit Type](10-unit type.md)

`(,)` returns a function that can be used to create a tuple. See creating tuple below.


### Creating tuples

Tuples are typically created by enclosing comma separated values in between parentheses.

```elm
$ elm repl
> (12, 7)
(12,7) : ( number, number' )
```

It is also possible to use a prefix operator, `(,)`, to construct a tuple. The function is a tuple constructor not bound to the value it helps create. 

```elm
$ elm repl
> (,) 12 7
(12,7) : ( number, number' )
> coord = (,)
<function> : a -> b -> ( a, b )
> coord 12 7
(12,7) : ( number, number' )
> coord 12 "7"
(12,"7") : ( number, String )
> (,,,) 1 True 'a' []
(1,True,'a',[]) : ( number, Bool, Char, List a )
```

### Operations on tuples

With tuples, type is bound to the number of elements. Therefore, functions have to target a given length. Different functions must be provided to append an element to a tuple pair, a tuple triple, or a tuple quad.

#### fst and snd of a tuple pair

For tuple pairs, special accessors are given that return the first or second element of the pair. 

```elm
$ elm repl
> pair = (5, 7)
(5,7) : ( number, number' )
> fst pair
5 : number
> snd pair
7 : number
> triple = (5, 7, 8)
(5,7,8) : ( number, number', number'' )
> fst triple
-- TYPE MISMATCH --------------------------------------------- repl-temp-000.elm
The argument to function `fst` is causing a mismatch.
Function `fst` is expecting the argument to be: ( a, b )
```

#### comparison

Tuples are comparable.

```elm
$ elm repl
> (5,7) == (5,7)
True : Bool
> (5,7) < (1,1)
False : Bool
> (5,7) < (10,10)
True : Bool
> (5,7) < (1,10)
False : Bool
> (5,7) < (10,1)
True : Bool
```

#### destructuring / pattern matching

Pattern matching in `case` expressions can be used on tuples. 

```elm
$ elm repl
> coord = (5,7)
(5,7) : ( number, number' )
> let (x,y) = coord in (x,y)
(5,7) : ( number, number' )
> (x, y) = (5, 7) -- doesn't work in the REPL.
-- NAMING ERROR ---------------------------------------------- repl-temp-000.elm
```

Destructuring can be used on nested tuples. 

```elm
$ elm repl
> nestedTuple = ("Jane", "Doe", (42, "female"))
("Jane","Doe",(42,"female")) : ( String, String, ( number, String ) )
> let (first, last, (age, gender)) = nestedTuple in first ++ " " ++ last ++ ", age: "++ (toString age) ++ ", gender: " ++ gender
"Jane Doe, age: 42, gender: female" : String
> let (first, last, _) = nestedTuple in first ++ " " ++ last
"Jane Doe" : String
```

Destructuring can be useful to declare constants within the elm program `(width, height) = (200, 100)`

Exact values of comparables can be used to match in a case of statement.

```elm
> isLastNameDoe tuple = \
 case tuple of \
   (_,"Doe")  -> True \
   (_,_)  -> False
<function> : ( a, String ) -> Bool
> isLastNameDoe ("Jane", "Doe")
True : Bool
> isLastNameDoe ("Albert", "Marb")
False : Bool
```
