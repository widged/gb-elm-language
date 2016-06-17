## Higher Order Functions

Pass functions as arguments to other functions.
```elm
double x = x * 2
List.map double [1..4] -- [2, 4, 6, 8]
List.map (\a -> a * 2) [1..4] -- [2, 4, 6, 8]
```


#### Currying

    concat : String -> String -> String
    concat a b = a ++ b

    listItem : String -> String
    listItem = concat "-> "


#### partial application

Partially apply a function by passing only some of its arguments. Then give that function a new name.

```elm
> minus x y = (-) x y
<function> : number -> number -> number
> minus1 = minus 1
<function> : number -> number
> minus1 11
-10 : number
```
